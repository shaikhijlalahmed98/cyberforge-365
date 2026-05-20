# Lab — Day 004: ShopFast AAA Audit

**Date:** 2026-05-20
**Linked lesson:** `lessons/2026-05-20-day4-aaa-authentication-authorization-accounting.md`
**Time:** 30–60 min
**Goal:** Har ShopFast endpoint ko teen A (Authentication, Authorization, Accounting) ke against audit karna; ek IDOR/BOLA gap pakadna + fix karna; ek non-repudiable audit-log schema design karna; aur ek JWT ke claims ko AuthN-vs-AuthZ mein classify karna.

> **Why this matters:** Aaj tu wahi reflex bana raha hai jo senior architect har PR review mein chalata hai — "is endpoint pe teeno A kaha hain?" Lab ka output portfolio evidence hai (ShopFast threat-model capstone, Day 30, mein reuse hoga).

---

## Setup — ShopFast endpoints (use these)

Yeh ShopFast ka simplified endpoint inventory hai. Isi pe kaam kar:

| # | Endpoint | Description |
|---|---|---|
| 1 | `GET /health` | Liveness probe |
| 2 | `POST /api/auth/login` | User login |
| 3 | `GET /api/orders/{id}` | Ek order ki detail |
| 4 | `GET /api/orders` | Logged-in user ke saare orders |
| 5 | `POST /api/refunds` | Refund issue karna |
| 6 | `GET /api/admin/users` | Saare users list (admin) |
| 7 | `PUT /api/users/{id}/role` | Kisi user ka role change |
| 8 | `POST /api/orders/{id}/invoice` | Invoice PDF download |

---

## Part A — AAA Mapping Table (≥ 8 rows, ~15 min)

Har endpoint ke liye yeh table bhar. **Default-deny** soch — pehle maan lo deny, phir justify karo allow.

| Endpoint | AuthN required? (Y/N + strength) | AuthZ coarse (roles/scopes) | AuthZ fine (object check?) | Accounting (audit event? 5 W's worth?) |
|---|---|---|---|---|
| `GET /health` | _e.g. N (public)_ | _none_ | _no_ | _no (noise)_ |
| `POST /api/auth/login` | | | | _Y — log success+FAIL attempts_ |
| `GET /api/orders/{id}` | | | _Y — ownership!_ | |
| `GET /api/orders` | | | | |
| `POST /api/refunds` | _Y — step-up MFA?_ | | | _Y — money movement_ |
| `GET /api/admin/users` | | _ADMIN_ | | |
| `PUT /api/users/{id}/role` | | | | _Y — privilege change = critical_ |
| `POST /api/orders/{id}/invoice` | | | _Y_ | |

**Success criteria:** Har row mein chaaron columns filled. Kam se kam 3 endpoints pe fine-grained (object ownership) authZ identify ki ho. Kam se kam 4 endpoints pe audit event maanga ho.

---

## Part B — Hunt the IDOR/BOLA Gap (~10 min)

Niche ek (deliberately buggy) ShopFast handler hai. Isay padh, bug identify kar, fix likh.

```csharp
// ShopFast — OrdersController (BUGGY)
[HttpGet("api/orders/{id}/invoice")]
[Authorize(Roles = "Customer")]      // coarse authZ done
public async Task<IActionResult> GetInvoice(int id)
{
    var order = await _orders.FindAsync(id);
    if (order is null) return NotFound();
    var pdf = await _invoices.RenderAsync(order);
    return File(pdf, "application/pdf");
}
```

**Tere answers (notes ya yahin likh):**

1. **Kaunsa A fail ho raha hai?** (AuthN / AuthZ-coarse / AuthZ-fine / Accounting)
2. **Attack:** Ek logged-in customer kaise doosre ka invoice nikaal sakta hai? (exact steps)
3. **Status code:** Deny pe kaunsa — 401 ya 403? Kyun?
4. **Fix:** Ownership check add karke handler dobara likh (5–8 lines).
5. **Accounting:** Is endpoint ke liye ek audit event line likh (5 W's). Denied-access bhi log ho?

> **Success criteria:** Tera fixed handler `order.CustomerId` ko current user (`User.FindFirstValue(ClaimTypes.NameIdentifier)`) se compare karta ho, aur mismatch pe `Forbid()` (403) deta ho. Audit event mein who/what/when/where/outcome ho.

---

## Part C — Design a Non-Repudiable Audit Schema (~10 min)

ShopFast ke liye ek audit-log schema design kar. JSON shape likh jo SIEM mein ja sake:

```json
{
  "event":     "REFUND_ISSUED",
  "who":       "user:Z12345 | role:Support",
  "what":      "POST /api/refunds order=ORD-998 amount=4999",
  "when":      "2026-05-20T14:07:33Z",
  "where":     "ip=203.0.113.7 trace=abc-123 device=...",
  "outcome":   "SUCCESS",
  "...":       "tu aur fields add kar"
}
```

**Tasks:**

1. Upar wale schema ko complete kar — 2 aur fields add kar jo non-repudiation mazboot karein.
2. **Teen rules likh** jo is log ko tamper-evident/non-repudiable banayein (hint: append-only, off-host, hash-chain, never-log-secrets).
3. **Teen cheezein list kar** jo is log mein **kabhi** nahi honi chahiye (PCI/secret hygiene).
4. Ek anomaly-alert rule English/pseudocode mein likh jo "Morgan Stanley-style" abuse pakde (hint: ek user baseline se N× zyada records access kare).

> **Success criteria:** Schema mein 5 W's + 2 extra fields. 3 tamper-evidence rules. 3 never-log items. 1 anomaly rule.

---

## Part D — JWT Claims: AuthN vs AuthZ (~10 min)

1. Browser mein `https://jwt.io` khol (ya `dotnet`/`jq` se decode kar — koi external service avoid karna ho to local decode).
2. Yeh sample ShopFast JWT payload le (ya apne dev token ka payload):

```json
{
  "sub": "Z12345",
  "name": "Ijlal Ahmed",
  "iss": "https://auth.shopfast.local",
  "aud": "shopfast-api",
  "iat": 1747748853,
  "exp": 1747752453,
  "roles": ["Customer", "Support"],
  "scope": "orders:read refunds:write"
}
```

**Tasks:**

1. Har claim ke saamne likh: yeh **AuthN claim** hai (kaun ho / token valid hai) ya **AuthZ claim** hai (kya allowed ho)?
   - `sub`, `iss`, `aud`, `iat`, `exp` → ?
   - `roles`, `scope` → ?
2. **`alg:none` attack** ko 2 lines mein samjha: agar API server signature validate na kare, attacker is token mein `"roles": ["Admin"]` daal de to kya hoga? Yeh kaunsa A toda?
3. **Defense:** Server-side 4 cheezein jo har JWT pe validate honi chahiye. (hint: signature, `iss`, `aud`, `exp`.)

> **Success criteria:** Har claim sahi categorize. `alg:none` ko authZ-bypass ke roop mein samjhaaya. 4 validation checks naam se.

---

## Part E — One-Paragraph Architect Recommendation (~5 min)

ShopFast ke CTO ko ek paragraph likh (4–6 lines): *"Humein abhi AAA mein sabse pehle kya theek karna chahiye aur kyun?"* — Morgan Stanley case ko tie-in kar (authentication theek hone ke bawajood authZ + accounting fail hua). Ek concrete pehla step recommend kar.

---

## Deliverables checklist

- [ ] Part A: AAA mapping table — 8 endpoints, chaaron columns
- [ ] Part B: IDOR identified + fixed handler + audit event + correct status code
- [ ] Part C: audit schema (5 W's + 2 extra) + 3 tamper rules + 3 never-log + 1 anomaly rule
- [ ] Part D: claims classified + `alg:none` explained + 4 validation checks
- [ ] Part E: CTO recommendation paragraph with Morgan Stanley tie-in
- [ ] (Bonus) Apne real project ke ek endpoint pe yeh poora audit chalaya

---

## Stretch goal (optional)

Apne local .NET ya Spring Boot project mein:
- Ek `AuditLogger` add kar jo structured JSON event likhe (who/what/when/where/outcome).
- Ek endpoint pe ownership check (fine-grained authZ) add kar.
- `[Authorize]` (AuthN only) vs `[Authorize(Roles=...)]` (coarse authZ) ka farq apni aankhon se dekh — pehle ko bypass kar ke (different user) IDOR demonstrate kar, phir fix kar.

Paste a screenshot / snippet of your audit log output as evidence.

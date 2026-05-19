# Lab — Day 003 — ShopFast Attack Surface & Blast Radius Map

**Time:** 45-60 minutes
**Deliverable:** This file, filled in with (a) an attack-surface inventory, (b) attack-vector mapping, (c) a blast-radius map from one assumed-compromised service, (d) real tool output from a surface scan, and (e) 3 blast-radius-reduction controls with snippets.
**Skills practiced:** Enumerating attack surface, distinguishing surface vs vector, reasoning about blast radius, writing least-privilege/segmentation controls.

---

## Background — ShopFast Inventory (same system, Day 2 se continue)

| Service | Tech | Internet-facing? | Data it touches |
|---|---|---|---|
| `WebFrontend` | Next.js 13 | ✅ Yes | Session cookies, user-visible data |
| `AuthService` | Spring Boot 2.7, Spring Security 5.7 | ✅ Via API gateway | Credentials, MFA tokens, password hashes |
| `OrderService` | .NET 6, PostgreSQL 13 | ❌ Internal only | Order records, addresses |
| `PaymentService` | .NET 6, Stripe integration, Log4j-wrapper | ❌ Internal only | Last-4 of cards, Stripe tokens |
| `RecommendationService` | Python 3.9, Apache Kafka 2.8 client | ❌ Internal only | Browsing history (no PII) |
| `AdminPortal` | Spring Boot 2.6, behind VPN+MFA, 6 users | ❌ Internal-VPN-only | Full DB read access for ops |

Assume: all 6 run as pods in one Kubernetes namespace `shopfast`, **flat network (no NetworkPolicy yet)**, and each pod has an AWS IAM role. Postgres is a managed RDS instance reachable from the cluster.

---

## Part A — Attack Surface Inventory (15 min)

Fill in the table. For each entry point, note the surface *type* (network / software / human / cloud-config) and whether it's internet-facing (north-south) or internal (east-west).

| # | Entry point | Service | Surface type | N-S or E-W | Authenticated? |
|---|---|---|---|---|---|
| 1 | `https://shopfast.com` (port 443) | WebFrontend | network+software | N-S | No (public) |
| 2 | `POST /api/auth/login` | AuthService | software | N-S | No (pre-auth) |
| 3 | _e.g., Stripe webhook endpoint_ | PaymentService | software | N-S | _your answer_ |
| 4 | _your answer_ | OrderService | | | |
| 5 | _your answer (hint: Actuator?)_ | AuthService/AdminPortal | | | |
| 6 | _your answer (hint: RDS 5432)_ | PostgreSQL | network | E-W | |
| 7 | _your answer (hint: cloud-config)_ | any pod | cloud-config | — | |
| 8 | _your answer (hint: human)_ | AdminPortal ops/vendor | human | — | |

> Aim for at least 8 distinct entry points across all 4 surface types. Don't forget the *hidden* surface: Actuator endpoints, the IMDS endpoint (`169.254.169.254`) reachable from every pod, the RDS port, and the human/vendor surface.

---

## Part B — Attack Vector Mapping (10 min)

For your 3 highest-value surface entries, name the **single most likely attack vector** and one control that blocks it.

| Surface entry | Most likely attack vector | Control that blocks it |
|---|---|---|
| `POST /api/auth/login` | _e.g., credential stuffing_ | _e.g., rate limit + MFA + breached-password check_ |
| _your pick_ | | |
| _your pick_ | | |

**Reflection (1-2 sentences):** Surface aur vector mein farq apne shabdon mein likh — is exercise ne kaise help ki distinction samajhne mein?

_Your answer..._

---

## Part C — Blast Radius Map (15 min)

**Assume `AuthService` gets compromised** (e.g., via an exploited dependency — pre-auth RCE). With the **current flat network + assume its IAM role has `s3:*` on `*` (Capital One-style)**, map the blast radius.

### C1 — Flat network blast radius

List everything the attacker can reach east-west and via cloud-config from the AuthService foothold. Draw an ASCII map or list:

```
AuthService (compromised)
  ├── east-west reachable (flat network, nothing blocks):
  │     ├── OrderService:8080      ? (yes/no + why)
  │     ├── PaymentService:8080    ? 
  │     ├── RecommendationService  ?
  │     ├── AdminPortal            ?
  │     └── PostgreSQL RDS:5432    ?
  └── cloud-config reachable:
        ├── IMDS 169.254.169.254   -> IAM creds for AuthService role
        └── with s3:* on *         -> ? buckets reachable
```

Fill in each `?` with yes/no and a one-line reason.

**Question:** Is blast radius mein sabse khatarnaak hop kaunsa hai aur kyun? (Hint: which single reachable target gives the attacker the most data?)

_Your answer..._

### C2 — Segmented + least-privilege blast radius

Now imagine you've applied: (1) default-deny NetworkPolicy with only `OrderService → PaymentService` and `* → AuthService` allowed, (2) IMDSv2 enforced, (3) AuthService IAM role scoped to only its own `shopfast-auth-config` bucket.

Re-draw the blast radius. How much smaller is it? Which hops are now blocked?

_Your answer..._

---

## Part D — Hands-On: Enumerate a Real Attack Surface (10-15 min)

Pick **ONE** of the three options below and paste your real output.

### Option 1 — `nmap` a local container (recommended)

Run a deliberately-vulnerable app locally and scan its surface (sandbox only — your own machine):

```bash
# Spin up OWASP Juice Shop (intentionally vulnerable, safe to scan)
docker run --rm -d -p 3000:3000 --name juice bkimminich/juice-shop

# Scan its open ports / services (your own container only!)
nmap -sV -p- localhost              # or: nmap -sV localhost
# Look at the HTTP surface headers (server fingerprint = recon surface)
curl -sI http://localhost:3000 | head -20

# Cleanup
docker stop juice
```

Paste: which ports were open? what server/version headers leaked? That leaked version IS attack surface (fingerprinting → known-CVE lookup, Day 2 skill).

```
<paste nmap + curl output here>
```

### Option 2 — Find an exposed Spring Boot Actuator surface

On any local Spring Boot app (or a test one), check what Actuator exposes:

```bash
curl -s http://localhost:8080/actuator | python3 -m json.tool
# If you see env / heapdump / mappings -> that's surface to lock down
curl -s http://localhost:8080/actuator/env | head -40
```

Paste what was exposed, then write the `application.properties` lines that lock it to `health` only on a separate `management.server.port`.

```
<paste output + your hardening config>
```

### Option 3 — Find an over-broad IAM policy (cloud-config surface)

If you have AWS CLI access to any sandbox/free-tier account:

```bash
# List roles, then inspect one role's inline/attached policies for wildcards
aws iam list-roles --query 'Roles[].RoleName' --output text
aws iam list-attached-role-policies --role-name <ROLE>
aws iam get-policy-version --policy-arn <ARN> --version-id <vN> \
  --query 'PolicyVersion.Document'
# Grep your output for the danger signs:  "Action": "*"   or   "Resource": "*"
```

Paste the policy and circle any `"*"` — that's blast radius waiting to happen.

```
<paste policy + note the wildcards>
```

---

## Part E — Three Blast-Radius-Reduction Controls (10 min)

Write/paste the actual config for **three** controls you'd ship for ShopFast. Use the lesson's snippets as a starting point but adapt to ShopFast service names.

### Control 1 — Default-deny NetworkPolicy + one allow rule

```yaml
# <your NetworkPolicy here — default-deny for namespace shopfast,
#  then allow only OrderService -> PaymentService:8080>
```

### Control 2 — Scoped IAM policy for one service (no wildcards)

```json
// <your least-privilege policy — e.g., PaymentService role limited to
//  its own bucket + Stripe secret in Secrets Manager only>
```

### Control 3 — Your choice (IMDSv2 enforce / non-root container / Actuator lockdown)

```
# <your snippet + one line why it shrinks blast radius or surface>
```

---

## Part F — Recommendation to the CTO (5 min)

Write a 5-7 sentence recommendation: given ShopFast's current flat network and (assumed) wildcard IAM roles, what are the **top 3 changes** you'd make first to shrink blast radius, in priority order, and why? Tie at least one to the Capital One lesson.

> **Recommendation:**
>
> _Your paragraph here..._

---

## Success Criteria

You've completed this lab when:

- [ ] Part A inventory has ≥8 entry points across all 4 surface types
- [ ] Part B maps vectors + controls for 3 high-value surfaces
- [ ] Part C has both flat and segmented blast-radius maps, with the worst hop identified
- [ ] Part D has **real** tool output (nmap / actuator / IAM) pasted in
- [ ] Part E has 3 concrete control snippets adapted to ShopFast
- [ ] Part F recommendation written, with a Capital One tie-in

Commit this file to `main` with:
```
Day 003 lab: ShopFast attack surface + blast radius map (Capital One lens)
```

---

## Bonus (Optional, 15 min) — Trace the IMDS path yourself

If you have any cloud VM (AWS free tier), from inside the VM run:
```bash
# IMDSv1 (the Capital One path) — does it return creds without a token?
curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/
# IMDSv2 (the fix) — requires a session token first:
TOKEN=$(curl -s -X PUT "http://169.254.169.254/latest/api/token" \
  -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
curl -s -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/iam/security-credentials/
```
Note whether v1 is still enabled (it shouldn't be). Document how IMDSv2's PUT-then-GET breaks the SSRF (GET-only) vector.

```
v1 enabled? ___    v2 working? ___    Why v2 blocks SSRF: ___
```

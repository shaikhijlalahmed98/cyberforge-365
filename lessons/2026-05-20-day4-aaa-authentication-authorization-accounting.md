# Day 004 — AAA: Authentication, Authorization & Accounting/Auditing

**Date:** 2026-05-20
**Phase:** Q1 → Month 1 → Week 1 (The Security Mindset)
**Theme:** Har darwaze pe teen sawaal — "Tu kaun hai?" (Authentication), "Tujhe ijaazat hai?" (Authorization), "Jo tune kiya uska record hai?" (Accounting/Auditing)
**Reading time:** ~60 min lesson + 45 min lab

---

## Table of Contents

1. The Story That Sets the Stage — Ek Bouncer, Ek Club, Teen Sawaal
2. Where AAA Came From — Dial-Up Modems Aur RFC 2865
3. The First A — Authentication ("Tu Kaun Hai?")
4. The Three Factors of Authentication
5. The Second A — Authorization ("Tujhe Kya Karne Ki Ijaazat Hai?")
6. AuthN ≠ AuthZ — The Distinction That Breaks Careers If Confused
7. The Third A — Accounting/Auditing ("Jo Tune Kiya Uska Record")
8. Non-Repudiation — Accounting Ka Superpower
9. .NET Core Mein AAA — Concrete Code
10. Spring Boot Mein AAA — Concrete Code
11. Anti-Patterns Table — "Mat Kar Yeh"
12. Mental Models You'll Carry Forward
13. Trade-offs and the Architect's Decision Framework
14. Beyond the Basics — Passkeys, SPIFFE, Policy-as-Code, AI Agent Non-Repudiation
15. Deep Incident — Morgan Stanley Insider Breach (2015)
16. Pattern Recognition — Equifax & Snowden Through the AAA Lens
17. Common Misconceptions
18. Glossary
19. Self-Check Quiz
20. Lab
21. Reflection & Tomorrow
22. Progress Entry

---

## 1. The Story That Sets the Stage — Ek Bouncer, Ek Club, Teen Sawaal

Ijlal, ek scene imagine kar. Ek high-end nightclub hai. Darwaze pe ek bouncer khada hai. Tu andar jaana chahta hai. Bouncer teen kaam karta hai, exactly is order mein:

1. **Pehle wo teri ID maangta hai.** "ID dikha." Tu apna driving license nikalta hai, photo match hoti hai, date of birth verify hoti hai. Bouncer ko ab yaqeen hai ke **tu wahi hai jo tu keh raha hai.** Is sawaal ka naam hai **Authentication** — "tu kaun hai, aur kya tu sach keh raha hai?"

2. **Phir wo guest list check karta hai.** Tu andar to aa gaya, par kya tujhe VIP lounge mein jaane ki ijaazat hai? Bartender ke peeche jaane ki? DJ booth mein? Bouncer tujhe ek wristband deta hai — green band matlab sirf general floor, gold band matlab VIP. Is sawaal ka naam hai **Authorization** — "tu kaun hai yeh to pata chal gaya, ab tujhe **kya karne** ki ijaazat hai?"

3. **Aur poore time, upar ceiling pe CCTV chal raha hai, aur entry register mein tera naam-time likha gaya.** Agar kal subah pata chale ke VIP lounge se ek mehngi bottle ghaayab hai, manager footage aur register dekhega: "raat 11:47 pe gold-band wala banda lounge mein gaya tha, naam Ijlal." Is record-keeping ka naam hai **Accounting** (ya **Auditing**) — "jo kuch hua uska tamper-proof record."

Bas. Yeh teen sawaal — **AAA** — har security system ki reet ki haddi (backbone) hain. Chahe wo ek nightclub ho, ek bank ka ATM ho, ya tera System-X2 ka `/api/payments` endpoint. Har request pe teen sawaal poochne padte hain:

- **Authentication (AuthN):** Tu kaun hai? (Identity prove kar.)
- **Authorization (AuthZ):** Tujhe is action ki ijaazat hai? (Permission check.)
- **Accounting/Auditing:** Jo tune kiya, uska non-repudiable record hai? (Log it.)

Day 1 pe humne **CIA Triad** seekhi (Confidentiality, Integrity, Availability) — wo *goals* the ("kya hum protect kar rahe hain"). Aaj ka AAA wo *mechanism* hai jo un goals ko enforce karta hai. AuthN + AuthZ milke Confidentiality enforce karte hain (sirf sahi log sahi data dekhein). Accounting Integrity aur detection ko support karta hai (agar koi cheez badle to record rahe). Aur Day 3 ke **blast radius** se seedha link: **least-privilege authorization blast radius ko chhota karta hai, aur accounting lateral movement ko detect karta hai.** Aaj sab kuch jud raha hai.

> **Mentor note:** Bahut se developers sirf pehle do A pe rukte hain — login (AuthN) aur roles (AuthZ). Teesra A — Accounting — wahi hai jo developer ko architect se alag karta hai. Kyun? Aage dekhega.

---

## 2. Where AAA Came From — Dial-Up Modems Aur RFC 2865

Chal thodi history, taake yeh teen letters hawa mein na lagein.

1990s mein internet dial-up se chalta tha. Tu phone line se ek modem ko call karta tha, aur ek ISP (Internet Service Provider) ka server tujhe internet pe chhodta tha. ISP ke paas hazaaron customers hote the, har ek ka username/password. Sawaal yeh tha: jab koi modem se connect kare, to ek central jagah se kaise verify karein ke (a) yeh banda valid customer hai, (b) isne kitne minute use kiye (taake bill bhej sakein)?

Livingston Enterprises ke ek engineer, **Carl Rigney**, ne 1991 ke aas-paas iska solution banaya: ek central server jo network access devices (modems, routers) ke saath baat kare aur teen kaam handle kare. Isay naam mila **RADIUS** — *Remote Authentication Dial-In User Service*. Yeh baad mein **RFC 2865** (year 2000) ke roop mein standardize hua. RADIUS ne pehli baar teen kaamo ko ek framework mein bandha:

- **Authentication:** username/password verify karo.
- **Authorization:** is user ko kaunse network resources milein (kitni speed, kaunse IPs).
- **Accounting:** session kitni der chali, kitna data transfer hua — billing aur audit ke liye record karo.

Yahin se **"AAA framework"** ka naam pada. Cisco ne apna version banaya — **TACACS+** (Terminal Access Controller Access-Control System Plus) — jo network device administration ke liye AAA deta hai aur AuthN/AuthZ ko alag-alag handle karta hai (RADIUS dono ko ek saath bundle karta hai). Aaj bhi tera enterprise WiFi (802.1X), VPN, aur network switches isi AAA model pe chalte hain.

> **Backend dev intuition:** Jo RADIUS ne 1991 mein modems ke liye kiya, wahi aaj tera API gateway + identity provider + SIEM milke karte hain. AuthN = Keycloak/Auth0 token issue karta hai. AuthZ = tera API gateway / OPA policy decide karta hai. Accounting = tera structured logs + audit trail SIEM mein jaate hain. Naam wahi, scale alag.

> **Takeaway:** AAA koi web-dev fad nahi — yeh 30+ saal purana, battle-tested network-access model hai jo har layer pe apply hota hai: network, OS, application, aur ab AI agents tak.

---

## 3. The First A — Authentication ("Tu Kaun Hai?")

Chal pehle problem samajh. Maan le tere paas ek bank ka counter hai. Ek banda aata hai aur kehta hai "main Ijlal hoon, mere account se 50,000 nikaal do." Tu kaise maane ke yeh sach mein Ijlal hai, koi imposter nahi? Tujhe **proof** chahiye. Yeh proof maangne aur verify karne ka process hi **Authentication** hai.

Formal framing (NIST SP 800-63 spirit): *Authentication* = "verifying the identity of a user, process, or device, often as a prerequisite to allowing access to resources in an information system." Aasaan lafzon mein: **claimed identity ko proof ke saath verify karna.**

Yahan ek nuance hai jo bahut log miss karte hain — **identification** aur **authentication** alag cheezein hain:

- **Identification** = "main Ijlal hoon" (ek claim, koi bhi keh sakta hai). Yeh tera username / email / user ID hai.
- **Authentication** = "aur yeh raha proof" (password, OTP, fingerprint). Yeh wo cheez hai jo sirf asli Ijlal ke paas honi chahiye.

Username public ho sakta hai (Ijlal ka email koi bhi guess kar le). Authentication factor secret hona chahiye. Isi liye "username enumeration" ek vulnerability hai par "username public hai" by itself disaster nahi — disaster tab hai jab authentication factor weak ho.

### Backend dev intuition

Tere ASP.NET Core app mein jab tu `[Authorize]` attribute lagata hai bina kisi role ke — wo sirf yeh check karta hai ke **banda logged-in hai ya nahi** (authenticated identity maujood hai). Wo abhi tak yeh nahi dekh raha ke banda *kya* kar sakta hai. Yeh pehla A hai, akela. Spring Boot mein `authenticated()` rule wahi kaam karta hai.

> **Takeaway:** Authentication ka kaam sirf itna hai: "yeh request kis verified principal ki taraf se aa rahi hai?" Bas. Iske aage kuch nahi. Jo log AuthN ko AuthZ samajh lete hain, wo IDOR jaise bugs banate hain (Section 6 dekh).

---

## 4. The Three Factors of Authentication

Authentication "proof" teen categories se aata hai. Yeh "factors" yaad rakhna — interview mein roz poocha jaata hai:

| Factor | Naam | Kya hai | Backend example | Kamzori |
|---|---|---|---|---|
| **Something you know** | Knowledge factor | Password, PIN, security question | DB mein bcrypt/Argon2id hash | Phishing, brute-force, reuse, leak |
| **Something you have** | Possession factor | Phone (TOTP/push), hardware key (YubiKey), smartcard | TOTP secret, FIDO2 credential | SIM-swap (SMS), device theft |
| **Something you are** | Inherence factor | Fingerprint, face, iris | Biometric template (device-side) | Spoofing, can't be "rotated" |

Ek factor akela kamzor hai. Jab tu **do alag categories** se factor maangta hai, usay **MFA (Multi-Factor Authentication)** kehte hain — password (know) + phone OTP (have). Dhyaan rakh: do **passwords** maangna MFA nahi hai, kyunki dono ek hi category (know) se hain. Asli MFA mein factors **alag categories** ke hone chahiye.

Ek aur emerging factor jo zero-trust mein important ho raha hai — **"somewhere you are"** (location/network context) aur **"something you do"** (behavioral biometrics, typing pattern). Yeh **risk-based / adaptive authentication** ki bunyaad hai: agar Ijlal Karachi se aam tareeke se login karta hai to password kaafi, par agar achanak 3 AM ko Russia se login attempt aaye to system extra factor maangta hai. Yeh "continuous authentication" ki taraf pehla kadam hai (Section 14).

> **Takeaway:** "MFA = do alag *categories* ke factors." Yeh ek line tujhe SMS-OTP ki kamzori se le ke passkeys tak sab samjha degi. SMS possession factor hai, par kamzor (SIM-swap), isiliye industry FIDO2/passkeys ki taraf ja rahi hai.

---

## 5. The Second A — Authorization ("Tujhe Kya Karne Ki Ijaazat Hai?")

Ab maan le bank counter pe Ijlal ki identity verify ho gayi (AuthN done). Banda asli Ijlal hai. Lekin Ijlal keh raha hai "mujhe **doosre customer** Saad ke account ka balance dikha do." Identity to sahi hai, par kya Ijlal ko *yeh action* karne ki ijaazat hai? Bilkul nahi. Yeh decide karna — "is verified identity ko is specific action/resource ki permission hai ya nahi" — **Authorization** hai.

Formal framing: *Authorization* = "the process of granting or denying specific requests to access resources or perform actions, based on an authenticated identity's permissions/policies." Aasaan: **AuthN ke baad — tujhe kya allowed hai?**

Authorization ke models (yeh aage Week 11 mein deep jaayenge, abhi sirf naam-introduction):

- **RBAC (Role-Based Access Control):** Permissions roles se judi hoti hain. "Finance role wale invoices dekh sakte hain." Tera classic `[Authorize(Roles="Finance")]`.
- **ABAC (Attribute-Based Access Control):** Permissions attributes pe based — user ka department, time, resource ka owner, location. "Sirf owner apna order edit kar sakta hai" — yeh ABAC-flavor hai.
- **ReBAC (Relationship-Based, Google Zanzibar style):** "Tu is document ka editor hai kyunki owner ne tujhe share kiya." Relationships pe based.
- **PoLP (Principle of Least Privilege):** Har model ke upar yeh golden rule — **har identity ko sirf utni permission do jitni strictly chahiye, na ek bit zyada.** Yeh Day 3 ke blast radius se seedha juda hai: kam privilege = chhota blast radius.

### Backend dev intuition

Yahan **do tarah ki authorization** hoti hai jo log gadbad karte hain:

1. **Coarse-grained (role/endpoint level):** "Kya is role ko `/api/admin/*` access hai?" — yeh middleware/attribute pe ho jaata hai.
2. **Fine-grained (object/row level):** "Kya yeh user *is specific* order ka maalik hai?" — yeh tera business logic mein hota hai, har object ke liye.

Sabse common breach — **Broken Object Level Authorization (BOLA / IDOR)** — tab hota hai jab developer pehla check (role) kar leta hai par doosra (object ownership) bhool jaata hai. `GET /api/orders/123` pe tu check karta hai "logged in hai? haan." Par tu yeh nahi check karta "kya order 123 isi user ka hai?" Attacker `123` ko `124` kar deta hai aur kisi aur ka order padh leta hai. Yeh OWASP API Top 10 ka #1 hai (API1:2023). Yaad rakh — yeh **authorization** failure hai, **authentication** nahi.

> **Takeaway:** Authorization sirf "kaunse roles" nahi — wo "kaunsa user kaunse *object* pe" bhi hai. Endpoint-level check kar liya matlab object-level check ho gaya — yeh sabse mehnga galat-fehmi hai backend mein.

---

## 6. AuthN ≠ AuthZ — The Distinction That Breaks Careers If Confused

Yeh section chhota hai par sabse important. Ratta maar le:

> **Authentication = "Tu kaun hai?" (identity)**
> **Authorization = "Tujhe kya karne ki ijaazat hai?" (permission)**

Ek classic galti: developer sochta hai "banda logged in hai (AuthN), iska matlab wo apna data hi maangega." **Galat.** Logged-in attacker bhi ek logged-in user hai. Wo apni valid identity se *doosre* ki cheez maang sakta hai. Authentication ne sirf yeh prove kiya ke wo *koi valid banda* hai — yeh nahi ke wo *is action* ke liye allowed hai.

HTTP status codes bhi yeh distinction encode karte hain, aur developers inhe ulta use karte hain:

- **401 Unauthorized** = actually yeh "**Unauthenticated**" hai — "tu kaun hai pata nahi, login kar." (AuthN failed.)
- **403 Forbidden** = "tu kaun hai pata hai, par tujhe yeh karne ki ijaazat nahi." (AuthZ failed.)

Naam history ki galti hai (401 ka matlab "unauthenticated" hai naam ke baghair), par distinction yaad rakh: 401 = AuthN problem, 403 = AuthZ problem.

> **Mentor note:** Interview mein agar koi puche "401 aur 403 ka farq?" — yeh AuthN vs AuthZ ka litmus test hai. Aur agar koi system har authorization failure pe 401 bhejta hai, wo bug hai jo attacker ko batata hai "endpoint exist karta hai, bas login chahiye."

---

## 7. The Third A — Accounting/Auditing ("Jo Tune Kiya Uska Record")

Ab aata hai wo A jise sab ignore karte hain — aur jiski ghair-maujoodgi ne arabon dollar ki breaches ko *mahine-mahine* tak chhupaaye rakha.

Chal problem samajh. Maan le tera ShopFast chal raha hai. Ek din pata chalta hai ke kisi ne 50,000 customers ka data nikaal liya. Pehla sawaal CEO poochega: **"Kisne nikaala? Kab? Kaunsa data? Kahan se?"** Agar tere paas in sawaalon ka jawab nahi — agar tere paas record hi nahi — to tu andhe ho. Tu nahi bata sakta kya gaya, kis customer ko notify karna hai, attacker andar abhi bhi hai ya nahi. **Accounting** wahi record hai jo yeh sawaal answer karta hai.

Formal framing: *Accounting* (RADIUS terminology) / *Auditing* (security terminology) = "recording what an authenticated, authorized identity actually did — when, from where, on what resource, with what outcome — in a tamper-evident manner." Aasaan: **har meaningful action ka non-repudiable log.**

Ek acche audit log entry mein "5 W's" hone chahiye:

1. **Who** — kaunsi identity (user ID, service account, session ID). NOT just "a user."
2. **What** — kaunsa action aur kaunsa resource (`DELETE /api/orders/123`, "exported 50k records").
3. **When** — precise timestamp (UTC, monotonic source se ideally).
4. **Where** — source IP, device, geo, request ID/trace ID.
5. **Outcome** — success ya fail? (Failed attempts bhi log karo — brute-force yahin dikhta hai.)

Yahan teen critical rules hain jo architect ko aate hone chahiye:

- **Log security-relevant events, not everything.** Login (success + fail), privilege changes, data export, admin actions, authorization denials, payment events. Noise mein signal mat dabao.
- **Logs ko tamper-proof rakho.** Agar attacker andar aa ke logs delete/edit kar le, to accounting bekaar. Solution: append-only storage, off-host shipping (logs turant ek alag SIEM/system pe bhej do), aur cryptographic integrity (hash chaining). Yeh Day 1 ki **Integrity** pillar hai logs pe apply hoti hui.
- **NEVER log secrets.** Passwords, full card numbers (PCI-DSS violation), tokens, session IDs plaintext mein log mat karo. Log "what happened," not "the secret used." Yeh sabse common mistake hai — audit log khud ek data-breach ban jaata hai.

### Backend dev intuition

Tu shayad already `ILogger` (.NET) ya SLF4J/Logback (Spring) use karta hai debugging ke liye. **Audit logging us se alag concern hai.** Debug logs developer ke liye hain, ephemeral, kahin bhi. Audit logs *security/compliance* ke liye hain — structured, immutable, retained (PCI-DSS: 1 year, kuch 6+ months hot), forwarded to SIEM. Inhe mix mat kar. Audit log ek alag, structured stream hai jisme har entry machine-parseable ho (JSON), taake SIEM mein query/alert ban sake.

> **Takeaway:** "Without accounting, a breach is invisible and undeniable becomes deniable." Pehle do A attack *rok* te hain (preventive). Teesra A attack *detect aur prove* karta hai (detective + non-repudiation). Architect dono chahiye.

---

## 8. Non-Repudiation — Accounting Ka Superpower

Ek lafz jo accounting ko sirf "logging" se upar utha deta hai: **non-repudiation.**

Problem samajh: maan le tera ek admin "Ali" production DB se ek table drop kar deta hai. Baad mein wo kehta hai "maine nahi kiya, kisi aur ne mera account use kiya hoga." Agar tere paas sirf ek normal log hai jo Ali bhi edit kar sakta tha, to tu *prove* nahi kar sakta ke Ali ne kiya. Ali "repudiate" (inkaar) kar sakta hai.

**Non-repudiation** = aisa proof jo banda inkaar na kar sake ke usne action kiya. Yeh achieve hota hai:

- **Strong authentication** (taake "kisi aur ne mera account use kiya" wala bahana kamzor ho — MFA, hardware keys).
- **Tamper-evident logs** (taake "log fake hai" wala bahana na chale — append-only, cryptographically signed/hash-chained).
- **Digital signatures** (Day 36 pe deep jaayenge — banda apni private key se action sign karta hai, sirf wahi sign kar sakta tha).

Yeh wahi cheez hai jo classic **CIA Triad** mein nahi thi, aur isi wajah se ek extended model — **Parkerian Hexad** (Donn Parker) — six elements add karta hai: Confidentiality, Integrity, Availability + **Possession/Control, Authenticity, Utility**. Aur AAA ka accounting pillar in newer elements (especially authenticity/accountability) ko enforce karta hai.

> **Futurist hook:** Jab AI agents tere systems mein actions lenge (refund issue karna, config change karna), to "kaunse agent ne, kis instruction pe, kis human ki taraf se yeh kiya" — yeh non-repudiable record har AI governance framework ka core ban raha hai. Accounting + non-repudiation 2027+ mein sabse hot topic hai. (Section 14.)

---

## 9. .NET Core Mein AAA — Concrete Code

Chal ab teeno A ko ASP.NET Core mein dekhte hain. Pehla — pipeline ka order matter karta hai: **`UseAuthentication()` hamesha `UseAuthorization()` se pehle** (pehle pata karo kaun hai, phir pata karo kya allowed hai).

```csharp
// Program.cs — order critical hai
app.UseAuthentication();   // A1: "Tu kaun hai?" — token validate, principal set
app.UseAuthorization();    // A2: "Tujhe ijaazat hai?" — policy/role check
// (Accounting middleware niche aata hai)
```

**A1 + A2 — controller pe:**

```csharp
[ApiController]
[Route("api/orders")]
public class OrdersController : ControllerBase
{
    private readonly IOrderRepository _orders;
    private readonly IAuditLogger _audit;   // A3 — alag concern, niche

    // [Authorize] akela = sirf AUTHENTICATION ("logged-in hona zaroori")
    // Roles add karte hi AUTHORIZATION bhi shuru ho gayi (coarse-grained)
    [HttpGet("{id}")]
    [Authorize(Roles = "Customer,Support")]
    public async Task<IActionResult> GetOrder(int id)
    {
        var order = await _orders.FindAsync(id);
        if (order is null) return NotFound();

        // ⚠️ FINE-GRAINED AUTHORIZATION — yahin IDOR rukta hai.
        // Coarse-grained ([Authorize]) ne sirf bola "yeh ek valid Customer hai".
        // Yeh check batata hai "yeh us SPECIFIC order ka maalik hai".
        var userId = User.FindFirstValue(ClaimTypes.NameIdentifier);
        if (order.CustomerId != userId && !User.IsInRole("Support"))
        {
            // 403, NOT 401 — banda authenticated hai, bas authorized nahi
            return Forbid();
        }
        return Ok(order);
    }
}
```

**A3 — Accounting (audit log), alag structured stream:**

```csharp
public record AuditEvent(
    string Who,          // user/service identity
    string What,         // action + resource
    DateTimeOffset When, // UTC
    string Where,        // source IP + trace id
    string Outcome);     // "Success" | "Denied" | "Error"

// Security events ko app debug-logs se alag, immutable sink pe bhejo
public sealed class AuditLogger : IAuditLogger
{
    private readonly ILogger<AuditLogger> _log;
    public void Record(AuditEvent e) =>
        // structured (JSON) — SIEM isay query/alert kar sake.
        // NOTE: kabhi password/token/full PAN log mat karna.
        _log.LogInformation("AUDIT {@AuditEvent}", e);
}
```

**Anti-pattern (labeled WRONG WAY) — JWT claim ko blindly trust karna:**

```csharp
// ❌ MAT KAR: client-supplied header se role uthana
var role = Request.Headers["X-User-Role"];     // attacker isay set kar dega
if (role == "Admin") { /* ... */ }             // total authZ bypass

// ❌ MAT KAR: JWT signature validate kiye baghair claims trust karna
// (alg:none attack — attacker token ke andar role:Admin daal dega)
// ✅ SAHI: middleware JWT signature + issuer + audience + expiry validate kare,
//          phir hi User.Claims pe bharosa karo.
```

> **Backend takeaway:** `[Authorize]` = AuthN. `[Authorize(Roles=...)]`/policies = coarse AuthZ. Object-ownership `if` = fine AuthZ. Audit logger = Accounting. Chaaron alag, chaaron zaroori.

---

## 10. Spring Boot Mein AAA — Concrete Code

Spring Security mein bhi wahi teen A, alag mechanisms:

```java
// SecurityConfig.java — A1 (authN) + A2 coarse (authZ) declaratively
@Bean
SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http
      .authorizeHttpRequests(auth -> auth
          .requestMatchers("/api/admin/**").hasRole("ADMIN")   // AuthZ (coarse)
          .requestMatchers("/api/orders/**").authenticated()   // AuthN only
          .anyRequest().denyAll())                              // default-deny!
      .oauth2ResourceServer(o -> o.jwt(Customizer.withDefaults())); // A1: JWT validate
    return http.build();
}
```

**A2 fine-grained — method security + object ownership:**

```java
@RestController
@RequestMapping("/api/orders")
public class OrderController {

    @GetMapping("/{id}")
    @PreAuthorize("hasAnyRole('CUSTOMER','SUPPORT')")   // coarse AuthZ
    public ResponseEntity<Order> getOrder(@PathVariable Long id,
                                          @AuthenticationPrincipal Jwt jwt) {
        Order order = orders.findById(id).orElseThrow();

        // ⚠️ FINE-GRAINED — IDOR yahin rukta hai
        String userId = jwt.getSubject();
        boolean isSupport = jwt.getClaimAsStringList("roles").contains("SUPPORT");
        if (!order.getCustomerId().equals(userId) && !isSupport) {
            // 403 Forbidden — authenticated but not authorized
            throw new AccessDeniedException("not your order");
        }
        return ResponseEntity.ok(order);
    }
}
```

**A3 — Accounting via a dedicated audit appender (alag se logback/SIEM):**

```java
// AuditService.java — security stream, app logs se alag
public void record(String who, String what, String where, String outcome) {
    // structured key-value -> JSON appender -> SIEM (immutable, off-host)
    auditLog.info("event=AUDIT who={} what={} when={} where={} outcome={}",
        who, what, Instant.now(), where, outcome);
    // ❌ Never: password, raw token, full card number is line mein
}
```

> **Backend takeaway:** Spring mein `authenticated()` = AuthN, `hasRole()`/`@PreAuthorize` = coarse AuthZ, `if (!owner)` = fine AuthZ, audit appender = Accounting. `.anyRequest().denyAll()` rakhna — default-deny zero-trust ka pehla qadam hai (Day 3 yaad hai).

---

## 11. Anti-Patterns Table — "Mat Kar Yeh"

| Anti-pattern | Kya galat hai | Sahi tareeka |
|---|---|---|
| **AuthN ko AuthZ samajhna** | "Logged in = apna hi data maangega" — IDOR/BOLA paida hota hai | Har object pe ownership check (fine-grained authZ) |
| **Client-side authorization** | UI mein admin button chhupaya, par API endpoint khula | Authorization hamesha **server pe**, UI sirf cosmetic |
| **Do passwords = MFA** | Dono same factor (know) — real MFA nahi | Do alag *categories* (know + have) |
| **SMS as primary 2nd factor** | SIM-swap se bypass | TOTP / FIDO2 / passkeys |
| **JWT claims bina signature validate kiye trust karna** | `alg:none`, token tampering | Signature + issuer + audience + expiry validate |
| **Audit logs editable / on-host only** | Attacker andar aa ke logs mita de | Append-only, off-host SIEM, hash-chained |
| **Secrets in logs** | Audit log khud breach ban jaata hai (PCI violation) | Log action, never the secret/PAN/token |
| **Failed logins log na karna** | Brute-force/credential-stuffing invisible | Failed AuthN events bhi audit karo + alert |
| **Over-broad roles ("everyone is Admin-ish")** | Blast radius huge (Day 3) | Least privilege, granular roles |
| **403 ki jagah har jagah 401** | Galat semantics + info leak | 401 = AuthN, 403 = AuthZ |

---

## 12. Mental Models You'll Carry Forward

**Model 1 — The Bouncer (AAA in order):** ID check (AuthN) → wristband/guest-list (AuthZ) → CCTV+register (Accounting). Hamesha isi order mein. Pehle kaun, phir kya-allowed, saath-saath record.

**Model 2 — "AuthN once, AuthZ every time, Accounting always."** Authentication request ke shuru mein ek baar (token issue/validate). Authorization *har action* pe (coarse + fine). Accounting *har meaningful event* pe. Jo log AuthZ ko "ek baar login pe" check karte hain, wo IDOR khol dete hain.

**Model 3 — "Preventive + Detective."** AuthN + AuthZ = preventive controls (Day 6 pe yeh taxonomy aayegi) — galat cheez hone se *rokte* hain. Accounting = detective control — galat cheez ho gayi to *pakadta* hai aur *prove* karta hai. Ek strong perimeter + andha CCTV = aadhi security. Tujhe dono chahiye.

**Model 4 — "401 vs 403 reflex."** Jab bhi access deny ho, khud se pooch: "yeh AuthN fail (kaun?) hai ya AuthZ fail (allowed?)?" Yeh reflex tujhe poore design mein clear-headed rakhega.

---

## 13. Trade-offs and the Architect's Decision Framework

Har A mein security-vs-usability trade-off hota hai:

| Decision | Zyada security | Zyada usability | Architect ka balance |
|---|---|---|---|
| **AuthN strength** | Hardware key + biometric har baar | Sirf password, lamba session | Risk-based: high-value action pe step-up MFA |
| **Session lifetime** | Short token, baar-baar re-auth | Lamba session, kam friction | Short access token + refresh rotation (Day 70) |
| **AuthZ granularity** | Fine-grained har object pe | Coarse roles only | Coarse for routes, fine for sensitive objects |
| **Accounting volume** | Sab kuch log | Sirf errors | Security-relevant events, structured, retained |

**Architect's decision framework (har naye endpoint/feature pe yeh 6 sawaal):**

1. **AuthN:** Is endpoint ko identity chahiye? (Public health-check = no. Baaki sab = yes.) Kitni strong? (Payment = step-up MFA.)
2. **AuthZ coarse:** Kaunse roles/scopes is endpoint ko hit kar sakte hain? Default **deny**, phir allow.
3. **AuthZ fine:** Yeh endpoint kisi *object* pe kaam karta hai? To ownership/relationship check zaroori (IDOR guard).
4. **Accounting:** Yeh action security-relevant hai? (Data read/write, privilege change, payment, admin.) Agar haan — audit event define kar (5 W's).
5. **Failure semantics:** AuthN fail → 401. AuthZ fail → 403. Dono log karo.
6. **Blast radius (Day 3 link):** Agar is identity ke creds churaye jaayein, kitna reachable hai? Least privilege se chhota kar.

> **Mentor note:** Yeh 6 sawaal har PR review mein apne dimaag mein chala. Senior architect feature dekh ke turant yeh checklist mentally run karta hai. Yahi "security review reflex" hai.

---

## 14. Beyond the Basics — Passkeys, SPIFFE, Policy-as-Code, AI Agent Non-Repudiation

Teeno A agle 3-5 saal mein kahan ja rahe hain:

**Authentication → Passwordless (Passkeys/FIDO2/WebAuthn).** Password fundamentally tooti hui cheez hai (phishable, reusable, leakable). Industry FIDO2/passkeys ki taraf ja rahi hai — cryptographic key pair jo phishing-resistant hai (private key device pe rehti hai, kabhi server pe nahi jaati). Apple/Google/Microsoft sab passkeys push kar rahe hain. 2027 tak yeh default ban jaayega consumer apps mein. (Day 64 pe deep.)

**Authentication for machines → SPIFFE/SPIRE & Workload Identity.** Microservices mein "service-to-service" auth abhi tak shared API keys pe chalta hai (jo Day 2 ke Uber GitHub leak jaise breaches deta hai). Future: har workload ko ek short-lived cryptographic identity milti hai — **SPIFFE** (Secure Production Identity Framework for Everyone) ek standard hai jo har service ko ek verifiable "SVID" deta hai, aur **mTLS** se services ek-doosre ko authenticate karte hain. API keys mar rahi hain. (Day 84.)

**Authorization → Policy-as-Code (OPA, Cedar) + Continuous Authorization.** Authorization rules code mein scattered hone ke bajaye, ek central policy engine mein declarative likhe ja rahe hain — **OPA (Open Policy Agent)** ya AWS ka **Cedar**. Aur "ek baar login pe check" se "continuous authorization" ki taraf — har request pe context (device health, location, risk score) dekh ke real-time decision. Yeh **Zero-Trust** ka core hai (NIST SP 800-207): "never trust, always verify." (Day 79, Day 182.)

**Accounting → Tamper-Evident Logs & AI Agent Non-Repudiation.** Logs ab cryptographically verifiable ban rahe hain (transparency-log style, hash chaining — Sigstore/Rekor jaisi tech). Aur sabse bada 2026+ frontier: **agentic AI**. Jab AI agents autonomous actions lenge (refund, deployment, config change), to "kaunse agent ne, kis prompt/instruction pe, kis human principal ki taraf se, kaunsa tool call kiya" — yeh non-repudiable audit trail har AI governance framework (NIST AI RMF, EU AI Act) ka requirement ban raha hai. Tera teesra A — accounting — AI security ka sabse demand-wala skill ban raha hai.

> **Futurist takeaway:** Classical AAA marega nahi — wo evolve karega. AuthN passwordless + workload identity, AuthZ policy-as-code + continuous, Accounting tamper-evident + AI-aware. Jo architect aaj AAA solidly samajhta hai, wo in sab transitions ko lead karega.

---

## 15. Deep Incident — Morgan Stanley Insider Breach (2015)

Yeh incident isliye chuna kyunki yeh AAA ke teeno A ko ek hi kahani mein dikhata hai — ek perfectly authenticated user, ek tooti hui authorization, aur accounting jo *bahut der se* signal de paya.

**Scene:** Morgan Stanley — duniya ke sabse bade wealth-management banks mein se ek. Ek financial advisor tha, **Galen Marsh**, jo legitimately employed tha. Uske paas valid credentials the, valid access tha. Authentication ka koi masla nahi tha — wo *exactly wahi tha jo hone ka claim karta tha*. Aur yahin AAA ka asli sabaq hai: **authentication theek hone se security guarantee nahi milti.**

**Timeline (compressed):**

| Date | Event |
|---|---|
| 2011–2014 | Marsh internal portals (Marsh ne "BIP" aur "3DX" naam ke 2 internal web apps use kiye) se ~730,000 accounts (≈350,000 clients) ka confidential data run/access kiya |
| Late 2014 | Kuch client data **online (Pastebin-style site)** pe sale ke liye post hua mila |
| Jan 2015 | Morgan Stanley ne breach announce kiya, Marsh ko terminate kiya |
| 2015 | Marsh ne federal court mein unauthorized computer access ka guilt plead kiya — 36 months probation + $600,000 restitution |
| June 2016 | **SEC** ne Morgan Stanley pe **$1 million** penalty lagaayi (Safeguards Rule violation); **FINRA** ne bhi $1M fine kiya |

**Numbers:** ~730,000 accounts touched; ~350,000 clients ka data exposed; $1M SEC penalty + $1M FINRA fine.

**Technical root cause (multiple layers):**

1. **Authorization failure (the core).** Morgan Stanley ke do internal applications mein access controls aise design the ke ek employee apne assigned clients se *bahut zyada* data run/query kar sakta tha — koi proper **need-to-know / least-privilege** enforcement nahi tha at the data layer. SEC ki finding ka dil yahi tha: authorization model business-need se decoupled tha. Yeh **fine-grained authorization** ka failure hai — exactly wo cheez jo humne Section 5/6 mein padhi.

2. **Accounting/monitoring lag.** Marsh ne hazaaron unauthorized queries mahino tak chalaayi. Audit logs *existed* (isi liye baad mein attribute ho paaya), par **proactive detection/alerting nahi tha** — koi anomaly alarm nahi baja jab ek advisor 730,000 accounts touch kar raha tha. Accounting "forensic after-the-fact" tha, "detective in-the-moment" nahi.

3. **No data-exfil controls (DLP).** Bulk data nikaalne pe koi rok/alert nahi.

**AAA breakdown:**

| Pillar | Status | Kya hua |
|---|---|---|
| **Authentication** | ✅ Worked | Marsh legit employee, valid creds — yahin trap hai |
| **Authorization** | ❌ Failed | Need-to-know enforce nahi tha; over-broad data access |
| **Accounting** | ⚠️ Partial | Logs the (post-hoc attribution hua) par real-time detection nahi |

**Architectural controls that would have prevented each layer:**

| Failure | Control jo rokta |
|---|---|
| Over-broad data access | Least privilege + need-to-know authZ at data/row level (ABAC: advisor sirf apne assigned clients) |
| Mahino tak undetected | UEBA / anomaly alert: "ek user ne baseline se 100x records access kiye" → real-time flag |
| Bulk exfil | DLP egress controls + rate limiting on data queries |
| Repudiation risk | Tamper-evident audit + strong identity → non-repudiation (yahan partly tha) |

**Architect's takeaway (quotable):**

> *"Authentication tells you the door was opened by a real key. It tells you nothing about whether that key should have opened that door. Insider threats are the purest proof that authorization and accounting matter more than authentication alone."*

**Sources:** SEC Press Release 2016-112 (June 8, 2016, "Morgan Stanley Failing to Safeguard Customer Data," $1M penalty, Securities Exchange Act Safeguards Rule / Regulation S-P); FINRA News Release (June 2016); US DOJ / SDNY filings on US v. Galen Marsh (guilty plea, 2015).

---

## 16. Pattern Recognition — Equifax & Snowden Through the AAA Lens

Ek hi incident kaafi nahi — pattern dekh:

**Equifax (2017, revisit Day 1 — ab Accounting lens se).** Day 1 pe humne dekha tha unpatched Apache Struts (CVE-2017-5638) ne entry di. Par attackers **76 din** tak data exfiltrate karte rahe *bina pakde*. Kyun? Equifax ke network-inspection device ka **SSL certificate 19 months se expired tha** — yani wo encrypted traffic ko decrypt/inspect hi nahi kar raha tha. Yeh **Accounting/detection failure** hai pure form mein: monitoring tha par andha tha. Plus, ek admin portal ke credentials **plaintext** mein store the, jisse 48 databases tak access mila — **authorization/least-privilege failure**. (Source: US GAO-18-559; US House Oversight Committee report, 2018.)

**Edward Snowden (2013, NSA — AAA textbook case).** Snowden ek sysadmin tha (authentication: legit). Par usne (a) colleagues ke credentials use kiye / over-broad sysadmin access tha — **authorization/least-privilege + PAM failure**, aur (b) NSA apne *own* internal admin activity pe proper logging/monitoring nahi rakhta tha — isiliye wo *theek se bata bhi nahi paaye ke usne kya-kya liya* — **catastrophic accounting failure**. (Source: US House Permanent Select Committee on Intelligence report, 2016; DoD reviews.)

**Common thread:** Teeno cases mein **authentication theek tha** (Marsh employee, Equifax ka portal cred valid, Snowden sysadmin). Breach **authorization** (over-broad access) aur **accounting** (blind/late detection) ke failure se hua. Yahi AAA ka sabse bada sabaq hai: **AuthN pe sab focus karte hain, par asli architecture-level damage AuthZ aur Accounting ke kamzor hone se hota hai.**

---

## 17. Common Misconceptions

| Misconception | Reality |
|---|---|
| "Authentication aur authorization same cheez hai" | AuthN = kaun ho; AuthZ = kya allowed ho. Bilkul alag. |
| "Logged-in user trusted hai" | Logged-in attacker bhi logged-in hai. Har object pe authZ check. |
| "MFA = do passwords" | MFA = do alag *categories* (know + have + are). |
| "401 = authorization fail" | 401 = AuthN (unauthenticated); 403 = AuthZ (forbidden). |
| "Logging matlab accounting ho gaya" | Audit log structured, immutable, off-host, non-repudiable hona chahiye — debug log nahi. |
| "Accounting optional hai, AuthN/AuthZ asli hai" | Bina accounting breach invisible + undeniable becomes deniable. Detective control zaroori. |
| "Authorization ek baar login pe check ho jaata hai" | AuthZ har action/object pe. AuthN once, AuthZ every time. |
| "Strong authentication = secure" | Insider/over-broad access (Marsh) batata hai AuthN akela kaafi nahi. |

---

## 18. Glossary

- **AAA:** Authentication, Authorization, Accounting — the three-question framework for access control (RADIUS origin, RFC 2865).
- **Accounting / Auditing:** Recording who did what, when, where, with what outcome — tamper-evident, non-repudiable.
- **ABAC (Attribute-Based Access Control):** Authorization based on attributes (department, owner, time, location).
- **Authentication (AuthN):** Verifying a claimed identity with proof. "Tu kaun hai?"
- **Authorization (AuthZ):** Granting/denying actions to an authenticated identity. "Tujhe ijaazat hai?"
- **BOLA / IDOR:** Broken Object Level Authorization / Insecure Direct Object Reference — accessing another user's object by changing an ID. OWASP API #1.
- **Claim:** A key-value assertion about an identity inside a token (e.g., `role: admin`, `sub: user123`).
- **Confused Deputy:** A privileged component tricked into misusing its authority on behalf of an attacker (SSRF is one form).
- **Factor (auth):** Category of proof — knowledge (know), possession (have), inherence (are).
- **FIDO2 / WebAuthn / Passkey:** Phishing-resistant passwordless authentication using public-key crypto.
- **MFA:** Multi-Factor Authentication — proof from two+ different factor categories.
- **Non-repudiation:** Proof so strong the actor cannot deny they performed an action.
- **Parkerian Hexad:** CIA + Possession/Control, Authenticity, Utility — extended security model.
- **PoLP (Principle of Least Privilege):** Grant only the minimum permissions strictly required.
- **Principal:** The authenticated entity (user, service, device) a request acts as.
- **RADIUS / TACACS+:** Network AAA protocols (RFC 2865 / Cisco) — origin of the AAA framework.
- **RBAC (Role-Based Access Control):** Permissions assigned via roles.
- **Step-up authentication:** Requiring stronger auth for higher-risk actions.
- **UEBA:** User & Entity Behavior Analytics — anomaly detection on identity behavior (would've flagged Marsh).

---

## 19. Self-Check Quiz

Answer in `notes/2026-05-20-day4-aaa-authentication-authorization-accounting.md` **without peeking**. Mark difficulty (E/M/H); galat ya peek kiya → ❌.

1. **(E)** Define authentication, authorization, and accounting — one sentence each. Which order do they run in?
2. **(E)** What's the difference between a 401 and a 403 status code, in AAA terms?
3. **(M)** "Two passwords is MFA." Kyun yeh galat hai? Real MFA ki definition do.
4. **(M)** Ek developer `GET /api/invoices/{id}` pe `[Authorize(Roles="Customer")]` lagata hai aur khush hai. Kaunsa AAA failure abhi bhi possible hai? Naam + fix.
5. **(M)** Audit log entry ke "5 W's" kaunse hain? Aur teen cheezein jo audit log mein **kabhi** nahi honi chahiye.
6. **(H)** Non-repudiation kya hai? Teen technical cheezein jo isay enable karti hain. Yeh CIA Triad mein kyun nahi thi?
7. **(H)** Morgan Stanley case mein authentication theek tha phir bhi breach hua — kis A ka failure tha aur kaunsa ek architectural control isay rokta? Apne shabdon mein.
8. **(H — ShopFast)** ShopFast ka `POST /api/refunds` endpoint design kar. Teeno A ke liye ek-ek concrete control likho (AuthN strength, AuthZ coarse+fine, audit event ke 5 W's).
9. **(H — Mera project)** System-X2 ka koi ek sensitive endpoint le. AuthN, AuthZ (coarse + fine), aur Accounting — abhi kya hai, kya missing hai? Honest bata.
10. **(H — Futurist)** API keys "mar rahi hain" — service-to-service authentication ka future kya hai (ek naam + ek line why)? Aur AI agents ke liye accounting kyun naya frontier hai?

---

## 20. Lab

➡️ **`labs/2026-05-20-day4-aaa-shopfast-audit.md`**

Aaj ka lab: ShopFast ke endpoints ka **AAA audit** karna — har endpoint ko teen A ke against map karna, ek IDOR/BOLA gap dhoondhna aur fix karna, ek non-repudiable audit-log schema design karna, aur ek JWT decode karke uske claims ko AuthN-vs-AuthZ mein classify karna. (30–60 min, portfolio evidence ban-ta hai.)

---

## 21. Reflection & Tomorrow

**Journal question (notes mein answer):**
> "Apne kisi production project ka ek sensitive action soch (refund, delete, export). Kya tujhe *aaj* bata sakte ho ke wo action **kisne, kab, kahan se** kiya — ek tamper-proof record se? Ya tere paas sirf debug logs hain jo developer edit kar sakta hai? Agar audit nahi hai, pehla audit event kaunsa add karega?"

**Connection prompt:**
> "Microservices mein tu service-to-service calls karta hai. Abhi wo ek-doosre ko *kaise* authenticate karte hain — shared API key? network trust ('andar wala safe hai')? Yeh AAA ke first-A (machine authentication) ka kaunsa weak pattern hai, aur SPIFFE/mTLS isay kaise theek karega?"

**Tomorrow preview:**
> **Day 5 → Defense-in-Depth.** Kal seekhenge: ek layer kabhi kaafi nahi. AAA ek layer hai — par agar wo fail ho jaaye to peeche kya hai? Multiple overlapping controls, har ek doosre ka backup. Capital One/Equifax dono single-layer failures the. Defense-in-depth wo philosophy hai jo "ek galti = game over" ko rokti hai.

---

## 22. Progress Entry

```
Day 004 | 2026-05-20 | Q1 | AAA: Authentication, Authorization, Accounting/Auditing | ✅ Done | Internalized AuthN≠AuthZ, the under-loved 3rd A (accounting/non-repudiation), Morgan Stanley insider case shows authZ+accounting > authN alone
```

---

#aaa #authentication #authorization #accounting #auditing #non-repudiation #idor #bola #least-privilege #mfa #passkeys #spiffe #zero-trust #morgan-stanley #equifax #shopfast

# Day 003 — Attack Surface, Attack Vectors & Blast Radius

**Date:** 2026-05-19
**Phase:** Q1 → Month 1 → Week 1 (The Security Mindset)
**Theme:** Attacker ke paas andar aane ke kitne darwaze hain (surface), kis raste se aata hai (vector), aur ek baar ghus jaaye toh kitni door tak phail sakta hai (blast radius)
**Reading time:** ~60 min lesson + 45 min lab

---

## Table of Contents

1. The Story That Sets the Stage — March 22, 2019
2. Why These Three Words Run Your Whole Career
3. Attack Surface — Saare Darwazon Ka Total
4. The Four Faces of Attack Surface
5. Attack Surface Reduction (ASR) — Kam Darwaze, Kam Tension
6. Attack Vector — Wo Ek Raasta Jo Attacker Choose Karta Hai
7. Surface vs Vector — The Distinction Architects Must Nail
8. Blast Radius — Ek Foothold Se Kitni Tabahi
9. Lateral Movement — Attacker Kaise Blast Radius Badhata Hai
10. Mental Models You'll Carry Forward
11. Trade-offs and the Architect's Decision Framework
12. Beyond the Basics — Where This Is Heading (Zero Trust, eBPF, AI agents)
13. Deep Incident — Capital One (2019)
14. Pattern Recognition — SolarWinds & SSRF Everywhere
15. Common Misconceptions
16. Glossary
17. Self-Check Quiz
18. Lab
19. Reflection & Tomorrow
20. Progress Entry

---

## 1. The Story That Sets the Stage — March 22, 2019

Ijlal, ek scene imagine kar. Capital One — America ka 5th-biggest credit card issuer, ek bank jo apne aap ko "tech company that happens to be a bank" kehta tha. Yeh log AWS pe poori tarah cloud-native chale gaye the, well ahead of most banks. Smart engineers, modern stack. Aur phir bhi, ek hi attacker — akeli, ek laptop se — **106 million logon ka data** utha le gayi.

Attacker ne koi zero-day exploit nahi khareeda. Koi insider nahi tha. Usne sirf teen cheezein notice kiin:

1. Internet pe ek **misconfigured firewall (WAF)** khula tha — yeh ek *darwaza* tha jo nahi hona chahiye tha. (← attack surface)
2. Us darwaze ke through usne server ko trick kiya ke wo apne hi andar ke ek secret address pe request bhej de — is trick ka naam **SSRF** hai. (← attack vector)
3. Us ek server ke credentials chura ke usne dekha — in credentials ke paas to **700+ S3 buckets** ka access tha. Ek server se poora data lake reachable tha. (← blast radius)

Yehi teen shabd aaj ka lesson hain. Aur dhyaan rakhna — yeh teeno alag-alag cheezein hain, log inhe mix kar dete hain:

- **Attack surface** = kitne darwaze hain (the *count* of exposure).
- **Attack vector** = attacker ne kaunsa raasta liya (the *path* taken).
- **Blast radius** = ek darwaze se andar aane par kitna kuch reachable/damageable hai (the *spread* of damage).

Day 1 pe humne CIA Triad (Confidentiality, Integrity, Availability) seekhi. Day 2 pe vocabulary — threat, vulnerability, exploit, risk — aur risk equation (`Risk = Likelihood × Impact`). Aaj hum us equation ke do hisson ko architecture mein convert karenge: **attack surface likelihood ko control karta hai, blast radius impact ko control karta hai.** Yani aaj ka lesson seedha kal ki risk math se juda hai.

---

## 2. Why These Three Words Run Your Whole Career

Ek backend developer "feature" ki language mein sochta hai: naya endpoint, naya integration, naya microservice, naya port. Ek security architect usi cheez ko ulta dekhta hai: **har naya feature ek naya darwaza hai jise defend karna padega.**

Yeh perspective shift hi tera asli kaam hai. Jab tu System-X2 mein ek naya `[HttpGet]` controller add karta hai, developer-brain kehta hai "ek feature ban gaya." Architect-brain kehta hai "attack surface +1, ab isko authenticate, authorize, rate-limit, log, aur monitor karna padega."

Teen reasons yeh teen words non-negotiable hain:

1. **Surface = likelihood ka driver.** Jitne zyada exposed darwaze, utni zyada chance ke koi ek galat configure ho jaaye (jaise Capital One ka WAF). Risk = Likelihood × Impact yaad hai? Surface seedha likelihood badhata hai.
2. **Blast radius = impact ka driver.** Ek service compromise hone ke baad agar attacker poora DB padh le, impact catastrophic. Agar wo ek isolated service tak hi simat jaaye, impact contained. Tu blast radius design karke impact ko shape karta hai.
3. **Yeh dono *design* decisions hain, *bug-fix* nahi.** Tu inhe code likhne ke baad patch nahi kar sakta easily — tu inhe architecture diagram pe pehle se decide karta hai. Isiliye yeh "architect" ka kaam hai.

> **Mentor note:** Agar koi tujhse interview mein puche "how do you think about security?" — yeh teen words bol de aur unka risk-equation se link bata de. Senior log yahin pe junior se alag dikhte hain.

---

## 3. Attack Surface — Saare Darwazon Ka Total

Chal pehle problem samajh. Purane zamaane mein ek **castle** hoti thi. Castle ke andar khazana. Castle ke bahar dushman. Defender ka kaam? Har wo jagah jahan se dushman andar aa sakta hai usko ginna aur guard karna: main gate, side gate, sewer tunnel, low wall, kitchen ki khidki. Jitni zyada aisi jagahein, utna mushkil defend karna. Is poori list — har possible entry point ka total — ko aaj hum **attack surface** kehte hain.

Formal definition (NIST framing): *attack surface* = "the set of points on the boundary of a system, a system element, or an environment where an attacker can try to enter, cause an effect on, or extract data from that system" (NIST SP 800-53 / NISTIR 8053 spirit).

Practical framing (architect lens): **attack surface = saare exposed entry points jahan attacker input de sakta hai ya interact kar sakta hai.** Har:

- Open port (`443`, `22`, `5432`...)
- HTTP endpoint / API route
- Form field, query param, header, cookie
- File upload
- Message queue topic jisme bahar se message aata hai
- Environment variable / config jo bahar se inject hota hai
- Third-party dependency (yeh transitive surface hai — Day 2 ka Log4Shell yaad hai?)
- Login page, password reset flow, OAuth callback
- Admin portal, debug endpoint, health check, metrics endpoint
- Cloud metadata endpoint (`169.254.169.254` — yaad rakh, yeh Capital One mein villain tha)
- Insaan (phishing target) aur uska device

…yeh sab attack surface ka hissa hai.

### Backend dev intuition

Soch is tarah: tera Swagger / OpenAPI spec literally tera attack surface ka ek hissa document karta hai. Har route jo Swagger mein dikhta hai, wo ek darwaza hai. Plus wo darwaze jo Swagger mein nahi dikhte (Spring Boot Actuator endpoints, Kestrel ka default `Server:` header, exposed DB port) — wo *hidden* surface hai, aur hidden surface sabse khatarnaak hai kyunki tu use bhool jaata hai.

> **Takeaway:** "Agar main is system ka Swagger + netstat + dependency tree ek saath dekhun, mujhe iska attack surface dikhega." Wahi list attacker bhi banata hai — sirf tujhse pehle.

---

## 4. The Four Faces of Attack Surface

Attack surface ek monolith nahi — iske chaar chehre hain. Architect ko chaaron alag-alag dekhne aate hone chahiye:

| Surface type | Kya include hota hai | ShopFast example | Reduce kaise karein |
|---|---|---|---|
| **Network attack surface** | Open ports, exposed services, load balancers, public IPs | `WebFrontend` aur `AuthService` internet-facing; `PostgreSQL` ka 5432 agar galti se exposed | Firewall, security groups, private subnets, close unused ports |
| **Software / application attack surface** | API routes, input fields, dependencies, business logic, file uploads | `/api/orders`, `/api/auth/login`, Stripe webhook, transitive libs | Input validation, remove unused endpoints/features, dependency hygiene |
| **Human / social attack surface** | Employees, support staff, vendors, anyone who can be phished or socially engineered | ShopFast ops team, the HVAC-style vendor with portal access | Security awareness, MFA, least privilege, vendor risk mgmt |
| **Physical attack surface** | Data centers, laptops, USB ports, badge access, office WiFi | Lost engineer laptop with prod creds; office network | Disk encryption, MDM, badge control (cloud era mein bahut chhota) |

Cloud era mein ek paanchwa chehra ubhar raha hai jo Capital One mein decisive tha — **cloud control-plane surface**: IAM roles, instance metadata service, S3 bucket policies, security group misconfigs. Yeh na network hai na pure software — yeh *configuration* surface hai, aur 2019 ke baad se yeh attackers ki favourite jagah ban gayi hai.

> **Mentor note:** Jab tujhe koi system handover ho, chaaron (panchon) surfaces alag-alag enumerate kar. Developers sirf software surface dekhte hain. Architect chaaron dekhta hai — aur cloud config surface ko sabse pehle.

---

## 5. Attack Surface Reduction (ASR) — Kam Darwaze, Kam Tension

Ab problem clear hai: zyada surface = zyada risk. Solution bhi seedha hai — **darwaze kam karo.** Is discipline ka naam hai **Attack Surface Reduction (ASR)**, aur iska core principle NIST isse kehta hai **"least functionality"**: *system sirf wahi expose kare jo strictly zaroori hai, baaki sab band.*

Yaad hai Day 2 ka Log4Shell? JNDI lookup feature kisi ko chahiye nahi tha, phir bhi default ON tha — wo extra surface tha jisne poori duniya jala di. Yeh ASR ka classic violation hai: **"jo feature use nahi ho raha, wo attack surface hai, asset nahi."** Developers ise YAGNI kehte hain (You Aren't Gonna Need It). Security mein YAGNI sirf clean code ke liye nahi — survival ke liye hai.

### Spring Boot — the Actuator trap (real anti-pattern)

Spring Boot Actuator debugging ke liye health/metrics/env endpoints deta hai. Bohot teams galti se yeh likh dete hain:

```properties
# ❌ ANTI-PATTERN — saare actuator endpoints internet pe expose
management.endpoints.web.exposure.include=*
management.endpoint.env.show-values=ALWAYS
```

Iska matlab `/actuator/env` (saare env vars + secrets!), `/actuator/heapdump` (memory dump), `/actuator/threaddump`, `/actuator/mappings` — sab reachable. Yeh ek hi line attack surface ko 10x kar deti hai. Sahi tareeqa:

```properties
# ✅ Least functionality — sirf health, aur wo bhi minimal detail
management.endpoints.web.exposure.include=health
management.endpoint.health.show-details=never
# Actuator ko alag management port pe daalo jo internet se reachable hi na ho
management.server.port=9001
management.server.address=127.0.0.1
```

### .NET — strip the defaults

ASP.NET Core Kestrel by default `Server: Kestrel` header bhejta hai (version fingerprinting = recon ka tohfa), aur agar tu careless hai toh swagger prod mein bhi chalu reh jaata hai:

```csharp
var builder = WebApplication.CreateBuilder(args);

// ✅ Surface reduction: server fingerprint header hatao
builder.WebHost.ConfigureKestrel(o => o.AddServerHeader = false);

var app = builder.Build();

// ✅ Swagger sirf non-production mein — prod mein yeh ek extra darwaza hai
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

// ✅ Sirf wahi endpoint map karo jo zaroori hai; debug/test endpoints prod build se hatao
app.MapGet("/api/orders/{id}", GetOrder).RequireAuthorization();

app.Run();
```

### Container surface — distroless

Ek aur jagah surface chhupti hai: container image. Agar tu `ubuntu:latest` base use karta hai, tere container ke andar `bash`, `curl`, `apt`, package manager — sab maujood. Attacker andar aaya toh uske paas ready-made tools mil gaye (yeh "living off the land" ka raw material hai). Solution: **distroless / minimal base** — sirf tera app aur runtime, koi shell tak nahi.

```dockerfile
# ❌ ANTI-PATTERN: poora OS = poora toolkit attacker ke liye
# FROM mcr.microsoft.com/dotnet/aspnet:8.0

# ✅ Distroless: no shell, no package manager, minimal binaries
FROM mcr.microsoft.com/dotnet/runtime-deps:8.0-jammy-chiseled AS final
WORKDIR /app
COPY --from=build /app/publish .
USER app                         # non-root: blast radius bhi kam (Section 8)
ENTRYPOINT ["./ShopFast.OrderService"]
```

### Common failure modes (surface ke leak hone ke 6 tareeqe)

1. **Forgotten debug/admin endpoints** prod mein chalu reh jaate hain (Actuator, Swagger, `/debug`).
2. **Default credentials** kabhi change nahi hote (router/admin/admin).
3. **Over-broad CORS** — `Access-Control-Allow-Origin: *` browser-side surface khol deta hai.
4. **Exposed databases** — `0.0.0.0` pe bind, public subnet mein Postgres/Mongo/Redis (Shodan pe lakhon milte hain).
5. **Unused dependencies** transitive surface laate hain (Day 2 ka sabaq).
6. **Shadow IT / orphaned assets** — wo purana staging server jo kisi ko yaad nahi, abhi bhi live hai.

> **Takeaway:** "Default ON" tera dushman hai. Architect ka mantra: **deny by default, expose by exception.**

---

## 6. Attack Vector — Wo Ek Raasta Jo Attacker Choose Karta Hai

Ab surface samajh aa gaya — saare darwaze. Lekin attacker saare darwazon se ek saath nahi aata. Wo *ek* raasta chunta hai. Us specific path/method ko **attack vector** kehte hain.

Definition (architect framing): **attack vector = the specific technique + path an attacker uses to gain access or deliver a payload.** Agar attack surface "kahan-kahan se aa sakte ho" hai, toh attack vector "is baar tum aaye kahan se aur kaise."

Common attack vectors (yeh tujhe MITRE ATT&CK mein "Initial Access" tactic ke neeche detail mein milenge — abhi sirf naam-pehchaan):

| Attack vector | Ek line mein | Real example |
|---|---|---|
| **Phishing** | Email/message se credentials ya malware delivery | 90%+ breaches yahin se shuru |
| **Exploited public app** | Internet-facing app ki vulnerability use karna | Log4Shell, Spring4Shell |
| **SSRF** | Server ko trick karke uske *andar* ki cheez access karna | **Capital One** (aaj ka incident) |
| **Stolen/valid credentials** | Leaked ya phished creds se seedha login | SolarWinds, countless breaches |
| **Supply chain** | Trusted dependency ya update channel ko poison karna | SolarWinds, Codecov, XZ Utils |
| **Exposed services / RDP** | Internet pe khula remote-access port | Ransomware crews ki favourite |
| **Misconfiguration** | Galat config se accidental exposure | Open S3 bucket, `exposure.include=*` |

SSRF (Server-Side Request Forgery) ko thoda detail dete hain kyunki aaj ka villain yahi hai. Story: web apps aksar server-side se URLs fetch karte hain — jaise "image URL do, hum thumbnail bana denge," ya "webhook URL do." Developer sochta hai user koi normal URL dega. Lekin attacker URL ki jagah `http://169.254.169.254/latest/meta-data/iam/security-credentials/` daal deta hai — yeh AWS ka **instance metadata** address hai jo server ke apne andar se reachable hota hai. Server bhola-bhala wo URL fetch karta hai aur attacker ko apne *khud ke* cloud credentials laa ke de deta hai. Server ko *forge* (jaali request banane pe) majboor kiya gaya — isiliye **Server-Side Request Forgery**. Iska weakness ID hai **CWE-918**.

> **Takeaway:** Surface tu design karta hai; vector attacker chunta hai. Tera kaam: har high-value surface ke saamne sochna "agar yahan se aaya toh kaunsa vector use karega?" — phir us vector ko block karna.

---

## 7. Surface vs Vector — The Distinction Architects Must Nail

Yeh confusion interview aur design review dono mein common hai. Saaf kar le:

| Pehlu | Attack Surface | Attack Vector |
|---|---|---|
| Sawal | *Kitne* aur *kahan* darwaze hain? | *Kis* raaste se aaya? |
| Nature | Static property of the system (design) | A specific path chosen (attacker action) |
| Measure | Count/extent (zyada/kam) | Method/technique (kaunsa) |
| Control | Surface *reduce* karo (kam darwaze) | Vector *block* karo (us raaste ko todo) |
| Analogy | Ghar ke saare entry points | Chor ne pichhli khidki se entry li |
| ShopFast | 6 services, 40 endpoints, 3 internet-facing | "AuthService ke login pe credential stuffing" |

Ek hi surface pe multiple vectors ho sakte hain. ShopFast ka `/api/auth/login` ek surface point hai; uske vectors: credential stuffing, SQL injection, brute force, phishing-stolen creds. Tu surface ek hi defend karta hai, par vectors kai sochne padte hain. Isiliye **defense-in-depth** (Day 5) aata hai — ek surface ke samne kai layers.

---

## 8. Blast Radius — Ek Foothold Se Kitni Tabahi

Ab sabse important — aur tere liye sabse intuitive — concept. **Blast radius.**

Tu microservices banata hai. Tujhe yeh shabd already pata hai, sirf doosre context mein. **Resilience engineering** mein blast radius matlab: "agar `PaymentService` down ho jaaye, kitni doosri services girengi?" Isiliye tu circuit breakers (Polly in .NET, Resilience4j in Spring), bulkheads, aur timeouts lagata hai — taake ek service ki failure poore system ko na le doobe.

**Security mein blast radius wahi concept hai, bas attacker ki nazar se:** "agar attacker *ek* component compromise kar le, wo wahan se kitna kuch reach, read, ya destroy kar sakta hai?"

Definition (architect framing): **blast radius = the total reachable/damageable scope from a single compromised identity, host, or component.**

Capital One mein blast radius catastrophic kyun tha? Kyunki jis WAF server ko attacker ne pop kiya, us server ki **IAM role** ke paas **700+ S3 buckets** ka read access tha. Ek server = poora data lake. Agar us role ke paas sirf 1-2 buckets ka scoped access hota (least privilege), toh same SSRF se sirf 1-2 buckets leak hote — 100 million records nahi.

Blast radius ko teen cheezein control karti hain:

1. **Network segmentation** — flat network (sab ek doosre se baat kar sakte hain) = bada blast radius. Segmented network (sirf zaroori connections allowed) = chhota blast radius. Microservices mein yeh "east-west traffic" control hai.
2. **Least privilege (IAM)** — har identity ke paas sirf utna access ho jitna uska kaam. Capital One ka asli sabaq.
3. **Secrets isolation** — har service ka apna credential/key. Agar sab ek shared DB user use karte hain, ek service compromise = poora DB. Per-service credentials = contained.

### North-south vs east-west (yaad rakhne wala model)

- **North-south traffic** = bahar se andar (internet → tera system). Yahan attack surface aur vectors live karte hain. Firewall/WAF yahan.
- **East-west traffic** = andar service-to-service (`OrderService` → `PaymentService`). Yahan **blast radius** live karta hai. Agar east-west pe koi control nahi (flat network), attacker ek service se baaki sab tak free ghoom sakta hai.

Purani architecture: hard perimeter (north-south guarded), soft interior (east-west wide open) — "M&M security: crunchy outside, soft inside." Modern attackers exactly isi soft interior ko exploit karte hain. Iska ilaaj **zero trust** hai (Section 12).

### Kubernetes — blast radius reduce karna (real config)

K8s mein by default har pod har doosre pod se baat kar sakta hai = flat network = poora blast radius. NetworkPolicy se "deny by default, allow by exception" karte hain:

```yaml
# ✅ Default-deny: koi bhi pod kisi se baat nahi kar sakta jab tak explicitly allow na ho
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
  namespace: shopfast
spec:
  podSelector: {}            # saare pods pe lagu
  policyTypes: [Ingress, Egress]
---
# ✅ Sirf OrderService ko PaymentService se baat karne do (aur kisi ko nahi)
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-order-to-payment
  namespace: shopfast
spec:
  podSelector:
    matchLabels: { app: payment-service }
  policyTypes: [Ingress]
  ingress:
    - from:
        - podSelector:
            matchLabels: { app: order-service }
      ports:
        - { protocol: TCP, port: 8080 }
```

Ab agar attacker `RecommendationService` pop kar le, wo `PaymentService` tak reach hi nahi kar sakta — blast radius us ek service tak simat gaya.

### AWS IAM — Capital One ka asli fix (least privilege)

```json
// ❌ ANTI-PATTERN: yehi galti Capital One ko le doobi (over-broad role)
{
  "Effect": "Allow",
  "Action": "s3:*",
  "Resource": "*"
}
```

```json
// ✅ Scoped: yeh role sirf order-archive bucket padh sakta hai, aur kuch nahi
{
  "Effect": "Allow",
  "Action": ["s3:GetObject", "s3:ListBucket"],
  "Resource": [
    "arn:aws:s3:::shopfast-order-archive",
    "arn:aws:s3:::shopfast-order-archive/*"
  ]
}
```

> **Takeaway (yeh ek line interview mein bol dena):** *"Attack surface reduction lowers the chance of getting in; blast radius reduction lowers the cost of getting in. Tu dono karta hai — kyunki ek din koi andar aa hi jaayega."*

---

## 9. Lateral Movement — Attacker Kaise Blast Radius Badhata Hai

Blast radius static nahi rehta — attacker use *grow* karta hai. Initial foothold (ek service) ke baad wo **lateral movement** (east-west pivoting) karta hai: ek host se doosre, ek identity se doosri, low-privilege se high-privilege (privilege escalation).

Capital One mein lateral movement step yeh tha: WAF server → IMDS endpoint → IAM credentials → S3 data lake. Har hop blast radius ko bada karta gaya. Agar in hops mein se *koi ek* block hota (segmentation, IMDSv2, scoped IAM), poora chain toot jaata. Yeh defense-in-depth ka core insight hai.

Backend intuition: lateral movement bilkul tere distributed transaction jaisa hai jisme ek service doosre ko call karti hai chain mein — bas yahan caller attacker hai aur har downstream call ek naya compromise. Tu jis tarah saga/choreography mein har step pe failure-handling sochta hai, security mein har east-west hop pe "kya yeh call authorized aur authenticated hai?" sochna hai.

---

## 10. Mental Models You'll Carry Forward

**Model 1 — "Doors & Rooms" (the house model).**
Attack surface = ghar ke darwaze/khidkiyan (kitne entry points). Attack vector = chor ne kis darwaze/khidki se entry li. Blast radius = andar aane ke baad kitne kamre bina chaabi ke khul jaate hain. Ek mehfooz ghar = kam darwaze (ASR) + har kamre pe alag taala (segmentation/least privilege). Capital One ka ghar: ek khuli khidki (WAF), aur andar aate hi *saare* kamre khule (over-broad IAM).

**Model 2 — "Surface controls likelihood, blast radius controls impact."**
Day 2 ka `Risk = Likelihood × Impact` yaad hai? Aaj usko architecture mein map kar:
```
        Likelihood  ── reduce karo via ── Attack Surface Reduction
Risk =      ×
        Impact      ── reduce karo via ── Blast Radius Reduction
```
Agar tu dono ghata de, risk dono dimensions mein girta hai. Yeh model tujhe har design review mein direction dega.

**Model 3 — "North-south vs East-west."**
North-south = perimeter (surface/vectors live here, firewall/WAF rakho). East-west = interior (blast radius lives here, segmentation/least-privilege rakho). Purani galti: north strong, east-west naked. Zero trust: east-west ko bhi north-south jaisa treat karo — har internal call bhi verify.

---

## 11. Trade-offs and the Architect's Decision Framework

### Trade-offs (kyunki har security control ki ek keemat hoti hai)

| Decision | Benefit | Cost / tension |
|---|---|---|
| Aggressive surface reduction | Kam exposure, kam recon | Dev velocity slow, debugging endpoints chhin jaate hain |
| Strict network segmentation | Chhota blast radius | Operational complexity, latency, debugging mushkil |
| Least-privilege IAM everywhere | Contained breaches | Initial setup slow, "access denied" tickets badhte hain |
| Distroless containers | Minimal software surface | `kubectl exec` se debug nahi kar sakte (no shell) |
| IMDSv2 enforce | SSRF se metadata theft block | Purane SDKs/scripts break ho sakte hain |

Architect ka kaam in trade-offs ko *consciously* tolerate karna hai, na ke convenience ke liye default ON chhod dena. "Security vs convenience" har din ki ladai hai.

### Architect's decision framework (step-by-step — har naye system pe chalao)

1. **Enumerate the surface.** Network (ports/SGs) + software (routes/deps) + human (vendors/staff) + cloud config (IAM/metadata/buckets). Sab likh.
2. **Mark internet-facing vs internal.** Internet-facing surface ko priority de — yahin se 80% breaches shuru hote hain.
3. **Reduce ruthlessly.** Har exposed point pe poochho: "Is this strictly needed?" Nahi? Band karo (least functionality).
4. **Per surface, list likely vectors.** Login → credential stuffing/injection. Upload → SSRF/malware. Webhook → SSRF/spoofing.
5. **Map blast radius for each foothold.** "Agar yeh service compromise ho, attacker kahan-kahan reach karega?" Diagram banao.
6. **Shrink blast radius.** Segmentation (NetworkPolicy/SG), least-privilege IAM, per-service secrets, non-root containers.
7. **Add detection on east-west + egress.** Capital One isiliye 4 mahine blind raha — koi alert nahi tha jab 700 buckets sync ho rahe the.
8. **Re-evaluate on every new feature.** Naya endpoint = surface +1 = is framework ko dobara chalao.

---

## 12. Beyond the Basics — Where This Is Heading

**Zero Trust Architecture (the default in 2025+).** Purana model: "perimeter ke andar = trusted." Naya model (NIST SP 800-207): **"never trust, always verify"** — har request, internal ya external, authenticate + authorize hoti hai. Iska seedha asar blast radius pe: agar har east-west call bhi verify hoti hai aur identity scoped hai, ek compromise se attacker kahin reach nahi kar sakta. Zero trust effectively blast radius ko ek service tak shrink karne ka philosophy hai.

**Microsegmentation + eBPF.** Ab segmentation IP-based firewall se identity-based ja raha hai. Tools jaise **Cilium** (eBPF-based) Linux kernel ke andar se pod-to-pod policy enforce karte hain — fast, identity-aware. eBPF blast-radius control ka future hai.

**Workload identity (SPIFFE/SPIRE) + service mesh mTLS.** Har service ko ek cryptographic identity (SPIFFE ID) milti hai, aur services aapas mein mutual TLS pe baat karti hain. Stolen network position se kuch nahi hota agar tere paas valid workload identity nahi. Yeh "identity is the new perimeter" ka workload version hai.

**IMDSv2 — Capital One ka direct architectural reply.** AWS ne 2019 ke baad **IMDSv2** banaya: ab metadata lene ke liye pehle ek session token PUT karna padta hai (SSRF aam taur pe sirf GET kar sakta hai), aur hop-limit credentials ko host se bahar jaane se rokti hai. Ek breach se poora cloud iska default badal gaya — yeh dikhata hai blast-radius lessons industry-wide kaise scale karte hain.

**Attack Surface Management (ASM/EASM).** Ab "tumhe pata bhi nahi tumhare kitne assets internet pe hain" ek $X billion problem hai. Tools (Shodan, Censys, commercial EASM) tere external surface ko continuously discover karte hain — kyunki shadow IT aur orphaned assets sabse bade darwaze hote hain.

**AI/agentic surface (2026 ka naya frontier).** LLM-powered apps ek bilkul naya attack surface laate hain: **prompt injection** (untrusted text instructions ban jaata hai), aur agentic AI mein **tool-calling se blast radius explode** hota hai — agar ek AI agent ke paas DB + email + filesystem tools hain aur usme prompt injection ho jaaye, ek malicious input ka blast radius poore toolset jitna ho jaata hai. Yani aaj ke yeh dono principles (surface reduction = agent ko kam tools do; blast radius = har tool ko scoped permission do) AI security mein bilkul same apply hote hain. Tu jaldi yeh dekhega.

---

## 13. Deep Incident — Capital One (2019)

> Primary incident for today. SSRF + over-broad IAM = textbook surface + vector + blast radius failure.

### Timeline (compressed)

| Date | Event |
|---|---|
| ~July 2018 onward | Capital One ka ek misconfigured open-source WAF (ModSecurity-based) EC2 pe internet-facing, SSRF ke against asurakshit |
| **Mar 22–23, 2019** | Attacker (Paige Thompson, ex-AWS engineer) SSRF se WAF ko trick karke IMDS (`169.254.169.254`) se WAF-role ke temporary credentials nikalti hai |
| Mar–Apr 2019 | Un credentials se `aws s3 ls` + `s3 sync` chalakar ~700 buckets ka data exfiltrate — koi alert nahi |
| **Jul 17, 2019** | Ek bahar ke researcher ko GitHub gist pe leaked data ke ishaare milte hain; Capital One ko responsible-disclosure email |
| **Jul 19, 2019** | Capital One internally confirm karta hai |
| **Jul 29, 2019** | Public disclosure + FBI attacker ko arrest karti hai (uske GitHub/Slack/Meetup handle se trace) |

### Numbers

- **~106 million** customers affected (~100M US + ~6M Canada).
- **~140,000 SSNs** aur **~80,000 linked bank account numbers** exposed; Canada mein ~1M SINs.
- Data ~700 S3 buckets se nikla.
- Cost: **$80M OCC civil penalty** (Aug 2020) + **$190M class-action settlement** (2021–22) + reputational damage.

### Technical root cause (the chain)

1. **Misconfigured WAF (surface):** internet-facing ModSecurity instance SSRF ke against vulnerable tha — server-side fetch ka misuse possible.
2. **SSRF (vector — CWE-918):** attacker ne WAF ko majboor kiya ke wo internal IMDS endpoint `http://169.254.169.254/latest/meta-data/iam/security-credentials/<role>` hit kare.
3. **IMDSv1 ne bina kisi session-binding ke credentials de diye:** koi check nahi ke yeh request legit application se hai ya forge ki gayi.
4. **Over-permissioned IAM role (blast radius):** WAF-role ke paas `ListBuckets` + S3 read across **sab** buckets tha — wo kaam jo ek WAF ko kabhi nahi chahiye tha. Ek server = poora data lake.
5. **No detection (response failure):** 700 buckets ka sync chala, lekin egress/anomaly alerting nahi tha. Breach external tip se pata chala, internal monitoring se nahi.

### CIA breakdown

- **Confidentiality:** catastrophically failed — 106M records exfiltrated.
- **Integrity:** primarily read access; data tamper ki report nahi, par capability maujood thi.
- **Availability:** directly affected nahi, par regulatory aur trust impact massive.

### Multiple root cause layers → architectural controls

| Failure layer | Kya galat tha | Architectural control jo rok deta |
|---|---|---|
| Surface | WAF SSRF ke against misconfigured + internet-facing | WAF hardening, SSRF input validation, allowlist outbound URLs |
| Vector | SSRF se IMDS reachable | IMDSv2 (session token PUT + hop-limit), egress allowlist (metadata IP block) |
| Privilege | WAF-role ke paas all-bucket S3 access | **Least-privilege IAM** — role sirf required buckets tak scoped |
| Data | Plaintext-readable buckets ek role se reachable | Bucket policies, VPC endpoints, per-dataset roles, tokenization of SSNs |
| Detection | 4 mahine blind, no egress alerting | GuardDuty/anomaly detection on mass S3 reads + egress monitoring |

### Architect's quotable takeaway

> *"Capital One ek hi galti se nahi mara — ek **chain** se mara. Lekin agar **koi ek** link bhi defense-in-depth ke tehat toot jaata (IMDSv2 hota, ya role scoped hota), 106 million records bach jaate. Blast radius reduction ka asli matlab: maan lo attacker andar aa gaya — phir bhi wo kitna kam le ja sake."*

### Authoritative sources

- DOJ press release & indictment, *United States v. Paige A. Thompson* (W.D. Wash., 2019).
- US Senate Homeland Security & Governmental Affairs Committee report on the Capital One breach (2022).
- OCC Consent Order & $80M civil money penalty (Aug 2020).
- Capital One official disclosure (Jul 29, 2019).
- Krebs on Security, "What We Can Learn from the Capital One Hack" (2019).
- MITRE **CWE-918: Server-Side Request Forgery (SSRF)**.
- AWS announcement & docs: **Instance Metadata Service Version 2 (IMDSv2)**.

---

## 14. Pattern Recognition — SolarWinds & SSRF Everywhere

**SolarWinds / SUNBURST (2020) — supply chain as attack vector, massive blast radius.** Attackers ne SolarWinds Orion ke *build pipeline* ko compromise kiya aur malicious code ko ek **trusted software update** mein daal diya (CVE-2020-10148 related). ~18,000 organizations ne wo signed update install kiya — ek hi vector (trusted update channel) ka blast radius **18,000 networks** ban gaya, including US government agencies. Sabaq: supply chain ek attack surface hai, aur trusted-channel ka blast radius poori customer base hota hai. Yahin se **SBOM, Sigstore, SLSA, attestation** (futurist supply-chain controls) ki demand aayi.

**SSRF as a recurring vector.** Capital One akeli nahi — SSRF OWASP Top 10 2021 mein naya entry bana (A10). Microsoft, GitHub, aur countless cloud apps SSRF se metadata-theft ke against hardened hue. Pattern: jahan bhi server user-controlled URL fetch karta hai, IMDS/internal services reachable ho sakti hain. Yehi reason IMDSv2 default ban gaya.

> **Pattern:** Surface ko underestimate karna (WAF "to bas WAF hai"), aur blast radius ko ignore karna (role "to bas chal raha hai") — dono milke har bade cloud breach ki recipe hain.

---

## 15. Common Misconceptions

| Misconception | Reality |
|---|---|
| "Attack surface aur attack vector same cheez hai." | Surface = saare possible entry points (count/extent). Vector = jo ek raasta attacker ne liya (method). |
| "Hamare paas WAF/firewall hai, surface covered hai." | WAF khud surface ban sakta hai (Capital One!). Perimeter tool ≠ surface elimination. |
| "Internal services ko secure karne ki zaroorat nahi, wo to firewall ke peeche hain." | Yehi "soft interior" galti hai. East-west = blast radius. Zero trust kehta hai internal ko bhi verify karo. |
| "Least privilege bas best-practice nice-to-have hai." | Least privilege blast radius ka primary control hai. Capital One mein iski absence = 106M records. |
| "Blast radius sirf resilience/SRE ki cheez hai." | Wahi concept security mein bhi central hai — bas attacker ki nazar se. |
| "Surface chhota karna matlab features kam karna = product weak." | Unused/exposed surface kam karna product weak nahi karta — wo sirf un darwazon ko band karta hai jo koi customer use hi nahi karta. |

---

## 16. Glossary

- **Attack surface:** Saare points (network, software, human, physical, cloud-config) jahan se attacker enter/interact/extract kar sakta hai. Count/extent.
- **Attack surface reduction (ASR):** Exposed surface ko minimize karne ki discipline; core principle = **least functionality**.
- **Least functionality:** System sirf wahi expose/run kare jo strictly zaroori hai; baaki sab band (NIST control).
- **Attack vector:** Wo specific path + technique jo attacker access/payload-delivery ke liye use karta hai.
- **Blast radius:** Ek compromised identity/host/component se total reachable ya damageable scope.
- **Lateral movement (pivoting):** Initial foothold ke baad attacker ka east-west spread — ek host/identity se doosre tak.
- **North-south traffic:** Bahar↔andar (internet↔system) traffic; yahan surface/vectors live karte hain.
- **East-west traffic:** Andar service-to-service traffic; yahan blast radius live karta hai.
- **Network segmentation / microsegmentation:** Network ko chhote isolated zones mein todna taake blast radius contained rahe; micro = per-workload granularity.
- **Least privilege:** Har identity ko sirf utna access jitna kaam ke liye zaroori — blast radius ka primary IAM control.
- **SSRF (Server-Side Request Forgery, CWE-918):** Server ko trick karke uske andar/intranet ki resources (jaise cloud metadata) fetch karwana.
- **IMDS / IMDSv2:** Cloud Instance Metadata Service (`169.254.169.254`) jo instance ko apne credentials/config deti hai; **IMDSv2** session-token + hop-limit se SSRF-theft rokta hai.
- **Zero Trust (NIST SP 800-207):** "Never trust, always verify" — har request (internal/external) authenticate+authorize; implicit interior trust khatam.
- **Distroless image:** Minimal container image bina shell/package-manager ke; software attack surface kam karta hai.
- **EASM / ASM (External/Attack Surface Management):** Continuously apne internet-facing assets discover & monitor karne ka practice/tooling.
- **Defense-in-depth:** Ek surface ke samne kai independent layers (Day 5 mein detail).

---

## 17. Self-Check Quiz

> Answer in `notes/2026-05-19-day3-attack-surface-vectors-blast-radius.md` **without peeking**. Mark each (E)asy / (M)edium / (H)ard.

1. Attack surface, attack vector, aur blast radius — teeno ko ek-ek line mein define kar, aur batao kaunsa risk-equation ke `Likelihood` se juda hai aur kaunsa `Impact` se.
2. Attack surface ke chaar (ya paanch) "faces" naam le. Cloud era mein kaunsa naya face decisive hai aur kyun?
3. "Least functionality" kya hai? Day 2 ke Log4Shell ko is principle se explain kar.
4. Spring Boot Actuator ki ek galat config line likh jo surface ko explode kar de, aur uska secure version. Kyun secure hai?
5. SSRF ko apne shabdon mein samjha (kyun "forgery"?). Capital One mein attacker ne SSRF se kaunsa internal address hit kiya aur kya nikala?
6. Capital One mein agar **sirf ek** control hota — IMDSv2 ya scoped IAM role — toh kya breach itna bada hota? Apna reasoning de (defense-in-depth lens).
7. North-south aur east-west traffic mein farq, aur batao blast radius kis mein live karta hai. Microservices mein east-west kaise contain karein?
8. **(ShopFast scenario)** Maan le attacker `RecommendationService` (Python, Kafka client, no PII) ko pop kar leta hai. Flat network mein blast radius kya hai vs default-deny NetworkPolicy ke saath? Tu kaun se 2 controls lagayega ke `PaymentService` reachable na ho?
9. **(Tera apna project)** System-X2 ya kisi past project ka ek internet-facing endpoint chun. Uski attack surface, ek likely attack vector, aur agar wo service compromise ho toh blast radius — teeno honestly likh. Kaunsa control sabse pehle add karega?
10. (H) "Zero residual risk possible nahi" — Day 2 ka sabaq. Toh attack surface 0 karna bhi possible nahi. Architect ka asli goal phir kya hai — surface aur blast radius dono ke liye ek-ek line mein?

---

## 18. Lab

> Full lab: [`labs/2026-05-19-day3-attack-surface-blast-radius.md`](../labs/2026-05-19-day3-attack-surface-blast-radius.md)

Aaj tu ShopFast ka **attack surface inventory + blast-radius map** banayega, ek real container pe `nmap`/`docker` se surface enumerate karega, aur 3 blast-radius-reduction controls (NetworkPolicy, IMDSv2, scoped IAM) propose karega snippets ke saath. ~45 min. Yeh tera doosra portfolio artifact banega.

---

## 19. Reflection & Tomorrow

### Journal question (answer in notes)

> "Apne kisi production project (System-X2 ya koi aur) ka network soch — kya wo 'crunchy outside, soft inside' tha (perimeter strong, internal services aapas mein freely baat karte the)? Agar ek service compromise hoti, blast radius kitna bada hota? Aaj ke baad tu sabse pehla east-west control kaunsa lagayega?"

### Connection prompt

> "Tu microservices mein resilience ke liye blast radius already control karta hai (circuit breakers, bulkheads). Wahi mental muscle security blast radius pe kaise apply hota hai? Resilience aur security blast-radius controls mein kya overlap hai, kya farq?"

### Tomorrow's preview

**Day 4 → AAA: Authentication, Authorization, Accounting/Auditing.** Aaj humne dekha attacker andar kahan se aata hai aur kitni door jaata hai. Kal seekhenge wo teen mechanisms jo har darwaze pe poochte hain: "tu kaun hai (authN)? tujhe yeh karne ki ijaazat hai (authZ)? aur jo tune kiya uska record hai (accounting)?" — yeh blast radius control ka identity-side foundation hai.

---

## 20. Progress Entry

```
Day 003 | 2026-05-19 | Q1 | Attack surface / vectors / blast radius | ✅ Done | Mapped ShopFast surface + blast radius; Capital One SSRF→IMDS→over-broad-IAM chain internalized
```

---

#attack-surface #attack-vector #blast-radius #ssrf #imdsv2 #least-privilege #segmentation #zero-trust #capital-one #microservices #dotnet #spring-boot

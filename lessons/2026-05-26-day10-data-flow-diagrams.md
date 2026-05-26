# Day 010 — Data Flow Diagrams (DFDs): Threat Modeling ka Asli Canvas

**Date:** 2026-05-26
**Phase:** Q1 → Month 1 → Week 2 (Threat Modeling Foundations)
**Theme:** Kal humne STRIDE seekha — 6 categories ka checklist. Par checklist *kis cheez pe* chalegi? Aaj woh "cheez" banate hain — **Data Flow Diagram (DFD)** — Shostack ke pehle sawaal *"what are we working on?"* ka asli tool. DFD woh canvas hai jis pe STRIDE letters pe-letter chalte hain.

---

## 📑 Table of Contents

1. [Today's Briefing](#1-todays-briefing)
2. [Core Concept — DFDs](#2-core-concept)
   - 2.1 Pehle problem: bina diagram threat modeling hallucination hai
   - 2.2 History: Yourdon/DeMarco se Shostack tak
   - 2.3 DFD ke 5 building blocks (with shapes)
   - 2.4 Trust boundaries — DFD ka security secret sauce
   - 2.5 DFD levels — L0 Context, L1, L2 (drill down)
   - 2.6 ShopFast L0 Context DFD — step-by-step walkthrough
   - 2.7 ShopFast L1 Checkout DFD — zoom in
   - 2.8 STRIDE-per-element ki shaadi DFD se
   - 2.9 DFD vs UML/sequence/architecture diagrams — kya farak
   - 2.10 Common DFD mistakes (anti-patterns)
3. [Mental Models](#3-mental-models)
4. [Trade-offs Analysis](#4-trade-offs-analysis)
5. [Architect's Decision Framework](#5-architects-decision-framework)
6. [Beyond the Basics — Futurist Lens](#6-beyond-the-basics)
7. [Real-World Incident — British Airways Magecart (2018)](#7-real-world-incident)
8. [Pattern Recognition — T-Mobile API breach (2023)](#8-pattern-recognition)
9. [Common Misconceptions Cleared](#9-common-misconceptions)
10. [Glossary](#10-glossary)
11. [Self-Check Quiz](#11-self-check-quiz)
12. [Lab Assignment](#12-lab-assignment)
13. [Reflection & Tomorrow](#13-reflection--tomorrow)
14. [Progress Entry](#14-progress-entry)

---

## 1. Today's Briefing

**Phase:** Q1 Foundations · **Month 1:** Security First Principles · **Week 2:** Threat Modeling Foundations · **Day 10 of 365**

**Why this matters TODAY:** Yaar, kal STRIDE-per-element ka concept dimaag mein pada — "External Entity pe S/R, Process pe full STRIDE, Data Flow pe T/I/D, Data Store pe T/R/I/D." Par ek imaandar sawaal: tu DEKHEGA kahan ke kaunsa element kahan hai? Agar tere paas system ka **picture** nahi, toh tu apne dimaag mein mental diagram banata rahega — aur woh diagram aksar **galat** hota hai (assumption-driven). Saare bade breaches mein ek pattern hai: **assumed data flow ≠ actual data flow**. British Airways ne socha tha card data sirf unke server tak jaata hai; *actual* mein woh attacker ke server pe bhi jaata tha. Agar woh data flow paper pe hota — drawn — woh missing arrow chillata. **DFD = systems ki dhundli mental tasveer ko sharp blueprint mein convert karne ka tool.** Aur sirf yahi ek tool hai jo guarantee deta hai ke tu poori surface dekh raha hai, sirf woh hissa nahi jo tujhe yaad hai.

**Connection to yesterday:** Day 9 mein STRIDE-per-element seekha — har element type pe kaunse letters lagte hain. Par "element" word abstract tha. Aaj woh elements physical shape mein nikalte hain — circle, rectangle, parallel lines, dashed line. DFD = STRIDE-per-element ka **execution surface**.

---

## 2. Core Concept

### 2.1 Pehle problem establish karte hain: bina diagram threat modeling = hallucination

Chal ek experiment kar. Aankhein band kar aur apne System-X2 ka ek complete picture dimaag mein bana — saari services, databases, message queues, third-party integrations, admin tools, batch jobs, reporting dashboard. Sab. **Ab imaandari se** — kya tu *sab* yaad rakh paya? Ya kuch components "background mein assume" ho gaye?

Chances are, kuch reh gaye:
- Woh ek-do legacy cron job jo abhi bhi production se DB padhta hai
- Woh debug endpoint jo developers ne 2 saal pehle add kiya tha aur production mein bhi enable hai
- Woh third-party analytics script jo har page pe inject hota hai
- Woh manual data export tool jo finance team istemal karti hai

Yahi gap **availability bias** hai (Day 9 mein STRIDE ke context mein bhi mila tha) — dimaag woh dikhata hai jo *recently touched* hai, sab nahi. Aur jo dimaag mein nahi hai woh threat modeling se *bhi* bahar hai. Tu STRIDE 6 letters jitna marzi chala, agar tujhe component pata hi nahi — koi mitigation possible nahi.

Solution kya? Wahi jo software engineering mein hamesha hota hai jab "memory reliable nahi": **external artifact bana de — kaagaz pe utaar.** Pilot checklist chalata hai. Surgeon WHO checklist. Architect blueprint banata hai. Threat modeler **DFD** banata hai.

DFD ek aisi tasveer hai jo system ka **runtime data flow** dikhata hai — kya component hai, data kahan se kahan jaata hai, kahan rukh ke store hota hai, kahan trust badalti hai. Tasveer banane ki act mein hi 30-40% threats nikal aate hain — kyunki "draw karne" se hi tujhe pata chal jaata hai konsi line tune **maani thi** (assumed) par actually hai *nahi* (ya ulta — exists par tune draw nahi ki).

> **Backend-dev intuition:** Tu microservices design karte waqt sequence diagram banata hai. Woh dikhata hai *kis order mein* calls jaati hain. DFD alag hai — woh dikhata hai *kya data flow karta hai aur kahan trust badalti hai*. Sequence diagram dev ka tool hai; DFD architect+security ka tool hai. Dono complement karte hain, replace nahi.

### 2.2 History: Yourdon/DeMarco se Shostack tak

Term ko backstory ke saath introduce karte hain — yahi yaad rakhega.

DFD ki paidaish security mein nahi hui. 1970s mein software engineering itni jawan thi ke log nahi jaante the "system" ko design *kaise* karein. Programs spaghetti hote the. Do consultants — **Tom DeMarco** aur **Edward Yourdon** — ne ek structured-analysis movement chalaya: "system ko data ke perspective se dekho — kahan se aata hai, kya transformations hote hain, kahan jaata hai, kahan rukta hai." Unhone ek visual language banayi: circles for processes, rectangles for external sources, arrows for data flows, parallel lines for stores. Yahi **classic DFD notation** ban gayi (1970-79).

Phir 2000s aaye. Microsoft Trustworthy Computing initiative (Day 8 reference) ke andar **Adam Shostack** aur team ne STRIDE ko teachable banaya, par ek practical problem face kiya: "STRIDE kis pe lagaayein?" Code pe? Architecture diagram pe? Words pe? Har approach mein gaps the. Unhone DeMarco/Yourdon ke **DFDs uthaye** aur ek **chhota addition** kiya — **trust boundary** (dashed line jo separate principals/security contexts dikhati hai). Yeh ek choti addition ne DFD ko **security threat modeling ka canonical tool** bana diya. Aaj Microsoft Threat Modeling Tool, OWASP Threat Dragon, IriusRisk — sab DFD-based hain.

Yeh continuity samajh — DFD security ke liye **invent nahi hui**, security ne **adopt** ki. Yahi reason hai woh aaj bhi sabse robust hai: 50 saal ka battle-tested grammar hai.

### 2.3 DFD ke 5 building blocks (shapes ke saath)

Sirf 5 cheezein hain — sab kuch inhi se ban-ta hai. Yaad rakh, kyunki kal lab mein draw karega.

```
┌──────────────┐         ◯           ═══════════              ─────────►       ┄┄┄┄┄┄┄┄
│              │       (        )    ═══════════              data flow        trust
│  external    │      ( process  )                                             boundary
│   entity     │       (        )    data store
└──────────────┘         ◯
```

**1. External Entity (Rectangle / Square)** — Tera system ke **bahar** ka koi actor jo data deta hai ya leta hai. Tera control nahi hai is pe. Examples: user (browser), third-party payment gateway, partner API, mobile app, admin in another system, scheduled cron from outside. **Yaad rakh: external = bahar tere trust boundary ke.** STRIDE pe yeh mostly **Spoofing aur Repudiation** ke targets hote hain (auth aur log proof).

**2. Process (Circle / Rounded box)** — Wahan jahan **transformation** hoti hai. Tera code chalta hai. Examples: Checkout Service, Payment Service, Order Validator, Auth Service. Yeh tere control mein hai (assuming on-prem/your cloud). Process pe **poora STRIDE** apply hota hai — har 6 letter sochna padta hai.

**3. Data Store (Parallel lines / Cylinder)** — Wahan jahan data **rukta** hai. Examples: PostgreSQL, Redis cache, S3 bucket, Kafka topic (debatable — kabhi flow, kabhi store), filesystem, ElasticSearch index, even an in-memory cache. Stores pe **T, R, I, D** hote hain — spoofing/EoP nahi (woh actor ki properties hain, store passive hai).

**4. Data Flow (Arrow)** — Data ek element se doosre tak kaise **travel** karta hai. HTTP request, database query, gRPC call, message bus publish, file transfer. Arrow ka **direction** matters. Hamesha **label** karna padta hai — kya cheez flow ho rahi hai (`{ cartId, items }`, `{ orderId, paymentToken }`). Flows pe **T, I, D** apply hota hai (tampering, info disclosure, DoS) — spoofing/repudiation/EoP nahi (flows passive hain).

**5. Trust Boundary (Dashed line)** — Ek border jo do alag **trust contexts** ko separate karti hai. Yahi DFD ka **security secret sauce** hai — agla section ispe pura focus.

### 2.4 Trust Boundaries — DFD ka security secret sauce

Yeh ek concept agar tu samajh gaya, threat modeling ka 50% kaam ho gaya.

**Trust boundary** = woh imaginary line jiske aar-paar **security context badalta hai**. Matlab: ek taraf jo data/code/actor pe tu bharosa karta hai (kuch hadd tak), doosri taraf jo *unknown ya partially trusted* hai. **Threats hamesha trust boundaries pe paida hote hain** — kyunki yahi woh jagah hai jahan attacker tere assumptions tod sakta hai.

Common trust boundaries (yaad rakh):

| Boundary Type | Example | Kya badalta hai |
|---------------|---------|-----------------|
| **Network boundary** | Internet ↔ DMZ ↔ internal LAN | Network reachability + identity assumptions |
| **Process boundary** | Service A ↔ Service B (microservices) | Memory isolation, process credentials |
| **User-privilege boundary** | Anonymous ↔ Authenticated user | Identity claims |
| **Role boundary** | Regular user ↔ Admin | Permissions |
| **Code-origin boundary** | First-party code ↔ Third-party library/script | Code provenance |
| **Compliance boundary** | PII zone ↔ analytics zone | Data classification |
| **Org boundary** | Your company ↔ vendor (Day 6 Target lesson) | Org-level controls |

Ek choti misal: Tu apne ghar mein nange paer chalta hai (high trust zone). Bahar gali mein nahi (low trust zone — joote pehnte ho). Bich mein **darwaza** trust boundary hai. Darwaze pe **lock** lagaate ho. Yahi trust boundary thinking hai — har boundary pe ek "lock" (control) chahiye.

DFD mein **dashed line** wahan kheechi jaati hai jahan trust badalti hai. Phir **har wo arrow jo dashed line ko cross karti hai** — woh ek **attack surface** hai (Day 3 ka attack vector — yaad?). Har crossing pe sawaal: "Yeh data/call inbound side ke liye trustworthy hai? Authenticate hua? Validate hua? Logged?"

**Backend-dev intuition:** Microservices mein har service-to-service call ek trust boundary cross karti hai (alag process, alag deployment, possibly alag node). Isi liye zero-trust architecture mein **mTLS + service identity + per-call authZ** default hota hai — har crossing pe lock.

### 2.5 DFD levels — L0 Context, L1, L2 (drill down ka art)

Ek mistake jo pehli baar DFD banane wale karte hain: poora system ek hi diagram mein thoonsne ki koshish. Yeh impossible aur unreadable hai. DFD **hierarchical** hai — alag-alag **levels** pe alag-alag detail.

- **Level 0 (Context Diagram)** — Sabse upar wala view. **Tera system ek hi process** (ek bada circle), aur baahar ke saare external entities + woh data jo flow karta hai. Goal: 60-second-view of "what is this system and who talks to it." Iske bina threat model start hi nahi hota.

- **Level 1** — L0 ka woh ek bada circle ab andar **decompose** hota hai 3-7 major sub-processes mein (microservices ya bounded contexts). Internal data stores aur internal flows ab visible hote hain. Trust boundaries appear hote hain.

- **Level 2 / Level 3** — Aur deep — ek L1 ka process andar se details. Yeh tab banate ho jab usi area mein deep dive chahiye (e.g., authentication subsystem ka L2 DFD).

**Rule of thumb (Shostack):** Ek diagram mein 7 ± 2 elements se zyada mat daal. Insaan ka working memory itna hi handle kar sakta hai. Zyada hua = decompose kar.

### 2.6 ShopFast L0 Context DFD — step-by-step walkthrough

Chal ab actually banate hain. ShopFast ek e-commerce platform hai (humara fictional system — Day 1 se chala raha hain). L0 mein "ShopFast" ek hi circle, aur uske aas-paas saare external entities.

```
                                     ┌─────────────────┐
                                     │  Payment        │
                                     │  Gateway        │
                                     │  (Stripe-like)  │
                                     └────────┬────────┘
                                              │  charge req / webhook callback
                                              ▼
                ┌──────────────┐         ╔═════════════╗         ┌──────────────┐
                │              │  HTTP   ║             ║         │  Shipping    │
                │   Customer   │◄──────► ║  ShopFast   ║◄──────► │  Partner API │
                │  (Browser)   │  REST   ║  (System)   ║  REST   │  (FedEx-ish) │
                └──────────────┘         ╚══════╦══════╝         └──────────────┘
                                                │
                                                │  audit + metrics
                                                ▼
                                     ┌─────────────────┐
                                     │  3rd-party      │
                                     │  Analytics      │
                                     │  (e.g., GA/Segment) │
                                     └─────────────────┘

                                          ▲
                                          │ admin actions
                              ┌───────────┴───────────┐
                              │   Admin (Employee)    │
                              │   via Admin Portal    │
                              └───────────────────────┘
```

**Kya seekha is L0 se?**
- 5 external entities: Customer, Payment Gateway, Shipping Partner, Analytics, Admin
- Sab Internet ke us paar hain — har arrow ek **trust-boundary crossing** hai
- Already 5 attack surfaces dikh rahi hain bina koi code dekhe
- "Analytics" wala arrow dhyan se dekh — yahi British Airways waala mauka hai (jab third-party JS first-party page pe load hota hai, code-origin boundary cross hoti hai aur log ignore karte hain)

### 2.7 ShopFast L1 — "ShopFast (System)" ko decompose karte hain

Ab us bade circle ke andar zoom karte hain. Microservices design jo Day 8/9 mein assume kiya tha:

```
                Customer (Browser)
                       │
                       │  HTTPS  (TLS)
              ╶ ╶ ╶ ╶ ╶│╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ Internet ↔ DMZ trust boundary
                       ▼
              ┌─────────────────┐
              │   API Gateway   │  (rate limit, TLS termination)
              │   (Process)     │
              └────────┬────────┘
                       │
              ╶ ╶ ╶ ╶ ╶│╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶ DMZ ↔ Internal LAN trust boundary
                       │
        ┌──────────────┼──────────────────┬───────────────────┐
        ▼              ▼                  ▼                   ▼
  ┌──────────┐  ┌──────────────┐  ┌──────────────┐  ┌────────────────┐
  │   Auth   │  │   Catalog    │  │   Checkout   │  │     Orders     │
  │ Service  │  │   Service    │  │   Service    │  │    Service     │
  │ (Process)│  │  (Process)   │  │  (Process)   │  │   (Process)    │
  └────┬─────┘  └──────┬───────┘  └──────┬───────┘  └────────┬───────┘
       │               │                 │                   │
       ▼               ▼                 │                   ▼
  ═══════════     ═══════════            │              ═══════════
  ║  Users  ║     ║ Catalog ║            │              ║ Orders  ║
  ║  DB     ║     ║   DB    ║            │              ║   DB    ║
  ═══════════     ═══════════            │              ═══════════
                                         │
                          ╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶│╶ ╶ ╶ ╶ ╶ ╶ ╶ ╶  Internal ↔ External Payment trust boundary
                                         ▼
                                ┌─────────────────┐
                                │ Payment Gateway │  (External Entity)
                                └─────────────────┘
```

Dekh ab kitni clarity aagayi. Ab tu STRIDE-per-element chala sakta hai:
- **External entity** (Customer, Payment Gateway): **S, R** sochna hai
- **Process** (API Gateway, Auth, Catalog, Checkout, Orders): har ek pe **STRIDE poora** (S,T,R,I,D,E)
- **Data flow** (har arrow): **T, I, D** sochna hai — khaas un arrows pe jo dashed line **cross** karti hain
- **Data store** (Users DB, Catalog DB, Orders DB): **T, R, I, D** sochna hai

Aur sabse mazedaar baat: **trust boundaries pe focus karte hi** tujhe pata chalega ke:
- API Gateway pe rate limiting (D), TLS (I), authN (S) zaroori hain
- Checkout → Payment Gateway flow pe signature verification (S, T) zaroori hai
- Sab DBs pe encryption at rest (I) + audit logs (R) zaroori hain

DFD ne 30 minute mein woh kaam kiya jo open-ended brainstorm hafton mein nahi kar sakta.

### 2.8 STRIDE-per-element ki shaadi DFD se

Day 9 mein woh table seekhi thi — ab DFD ke saath uska shaadi-haal:

| Element Type | DFD Shape | Applicable STRIDE | Reason (1-line) |
|--------------|-----------|-------------------|-----------------|
| External Entity | Rectangle | **S, R** | Identity (kaun hai?) + accountability (kya kiya?). Tampering/EoP ka concept hi nahi (you don't control them). |
| Process | Circle | **S, T, R, I, D, E** (sab) | Tera code = full attack surface. Sab kuch ho sakta hai. |
| Data Store | Parallel lines | **T, R, I, D** | Data tamper, audit gum, leak, DB down. (S/E actors ke liye hain, store passive). |
| Data Flow | Arrow | **T, I, D** | In-transit modification, sniffing, flooding. (S/R/E flows nahi, endpoints hain.) |
| Trust Boundary | Dashed line | **(flag for all crossings)** | Yeh element nahi, *amplifier* hai — har crossing ko 2x scrutiny do. |

**Mental model #2 (carry forward):** *"Pehle DFD, phir STRIDE."* DFD bina STRIDE = "checklist without inventory." STRIDE bina DFD = "memorized list without focus." Donon ek saath = poori machine.

### 2.9 DFD vs UML / Sequence / Architecture diagrams — kya farak

Common confusion — har diagram alag kaam karta hai:

| Diagram | Kya dikhata hai | Audience | Security ke liye? |
|---------|-----------------|----------|-------------------|
| **DFD** | Runtime **data movement** + trust contexts | Architects, security | ✅ Primary tool |
| **Sequence diagram** | Calls ka temporal order (kaun pehle, phir kaun) | Devs, integration | Partial (good for time-based threats like races/replay) |
| **Architecture diagram** | Components + deployments (often vague) | Stakeholders | Weak (no trust info usually) |
| **C4 Container/Component** | Hierarchical breakdown of components | Devs, architects | Decent — can be promoted to DFD |
| **ER diagram** | Data **structure** at rest | DB designers | Weak — no flow |

DFD ka unique selling point: **explicit trust boundaries + data labeled on flows**. Yeh kahin aur nahi milta.

### 2.10 Common DFD Mistakes (Anti-Patterns)

Pehli baar jo galtiyan sab karte hain — tu mat karna:

| ❌ Anti-pattern | 🔁 Why it's wrong | ✅ Correct approach |
|----------------|-------------------|---------------------|
| Control flow draw karna ("if user is admin, then...") | DFD = data flow, not logic. UML sequence diagram alag hai. | Sirf data movement dikha. Logic process ke andar implicit hai. |
| Trust boundary skip kar dena | "All my services trust each other" = wrong assumption (zero-trust mindset) | Har process/network/origin boundary draw karo. |
| Sab kuch ek diagram mein thoonsna | Unreadable, miss-prone | L0 → L1 → L2 — decompose. Rule of 7 ± 2. |
| Flow ko label na karna | "ek arrow" se threat assess nahi ho sakta | Har flow pe likho: `{ orderId, items, total }` |
| Third-party scripts/CDN ko bhool jaana | Sabse common — yahi British Airways ka lesson hai | Browser-side JS dependencies bhi external entities hain. |
| Admin/internal tools draw na karna | Admin paths sabse high-blast-radius hote hain | Admin portal, debug endpoints, internal cron — sab dikhao. |
| DFD ko one-time deliverable banana | System badalta hai, DFD purana ho jaata hai | DFD living document hai. Har major release pe update. |
| Logs/observability data flow chhupana | Logs me bhi PII leak ho sakta hai (Day 8 ka logging anti-pattern) | Log flows bhi draw karo — Splunk/ELK ek external entity ya store hai. |

---

## 3. Mental Models

1. **"Picture before checklist"** — DFD pehle banta hai, STRIDE baad mein lagti hai. Jaise pilot pehle flight plan banata hai, phir takeoff checklist chalata hai.

2. **"Threats live on the dashed lines"** — Trust boundary crossings hi attack surfaces hain. Agar tu time-pressed hai, sirf un arrows pe focus kar jo dashed lines cross karti hain — 80% threats wahin honge.

3. **"Assumed flow ≠ actual flow"** — DFD ka pehla draft hamesha tera *assumed* flow hota hai. Asli value tab milti hai jab tu ek senior dev ke saath review karta hai aur woh kehta hai "lekin yeh data toh actually X se bhi jaata hai" — yahi missing arrows tere blind spots hain.

4. **"Decompose until each process fits in one head"** — Agar tu ek process ko 1 sentence mein describe nahi kar sakta, woh aur decompose hona chahiye.

---

## 4. Trade-offs Analysis

| Approach | Pro | Con | Kab use karein |
|----------|-----|-----|----------------|
| **Hand-drawn DFD (whiteboard/Excalidraw)** | Fast, easy to revise, collaborative | Hard to version control, easy to lose | Pehla draft, design workshops |
| **OWASP Threat Dragon (free, web)** | Built-in STRIDE auto-threats, JSON-backed | Less polish than commercial | Solo projects, ShopFast |
| **Microsoft Threat Modeling Tool** | Powerful, enterprise patterns | Windows-only, dated UI | Microsoft shops, mature TM practice |
| **IriusRisk / SD Elements** | Auto-threat generation, compliance mapping | Expensive, enterprise license | Large orgs with mature SDL |
| **Threat-modeling-as-code (pytm, Threagile)** | Versionable, fits CI/CD, repeatable | Steeper learning curve | DevSecOps mature teams (futurist preferred) |

**Architect's rule:** Start with whiteboard/Excalidraw — banao, baith ke discuss karo, throw away. Phir jab system stable ho, OWASP Threat Dragon ya pytm pe lift kar lo for permanence.

---

## 5. Architect's Decision Framework

Jab bhi naya system / major feature aaye, yeh 6-step ritual:

1. **Scope set kar:** "Kya is feature/system mein hai aur kya nahi?" Boundaries chillenge.
2. **L0 banao:** Ek bada process + saare external entities + labeled flows. 5 minute mein.
3. **Trust boundaries draw karo:** Network, process, privilege, code-origin — sab. Dashed lines.
4. **L1 mein decompose karo:** Andar ke 3-7 sub-processes + their data stores + internal flows.
5. **STRIDE-per-element chalao** (Day 9): Har element par applicable letters likho — at least 1 threat per applicable letter.
6. **Prioritize + record:** Top threats ko risk register mein (Day 7 lab) le ja, treatment plan ke saath.

**Iterate:** DFD living document hai. Har major architectural change pe ek 20-minute "DFD diff" session — kya naya box aaya? Naya flow? Naya trust boundary? Naya threat?

---

## 6. Beyond the Basics — Futurist Lens

DFDs ka future bohot exciting hai — 3 directions:

1. **Threat-modeling-as-code (TMaC):** Manual diagramming dies. Aaj tools:
   - **OWASP pytm** (Python) — code mein DFD likho, threats auto-generate
   - **Threagile** (Go, YAML) — same idea, YAML-driven
   - **IaCBot** — Terraform/CDK se DFD reverse-engineer karta hai
   - **STRIDE-GPT** — LLM-assisted threat draft

   2027+ vision: tu Terraform/Pulumi commit karega, CI mein DFD auto-generate hoga, STRIDE rules check karenge, PR comments mein threats post honge. Threat modeling = pre-merge gate.

2. **DFD for AI/LLM systems:** Classical DFD doesn't capture "training data → model weights → inference outputs" lifecycle. MITRE **ATLAS** aur OWASP **LLM Top 10** new flow types add kar rahe hain — prompt context flow, RAG retrieval flow, tool-use flow (agentic AI). Agle 12-18 mahine mein "AI-aware DFD notation" emerge hogi.

3. **DFD for zero-trust mesh:** Service mesh (Istio/Linkerd, Day 109) mein har service-to-service call ek implicit trust boundary hai. **eBPF-based observability** (Cilium Hubble) ab runtime mein **actual** data flows capture karti hai — yani DFD ko **observed**, not just **designed**. Tera assumed DFD vs runtime actual DFD ka diff = drift detection.

---

## 7. Real-World Incident — British Airways Magecart (2018)

### Timeline (compressed)

| Date | Event |
|------|-------|
| Aug 21, 2018 | Attackers inject 22 lines of obfuscated JavaScript into BA's website (ba.com) — payment-form skimmer |
| Aug 21–Sep 5, 2018 | Skimmer harvests card data from ~380,000 transactions in real-time, exfiltrates to attacker domain `baways.com` (homoglyph-typosquat) |
| Sep 5, 2018 | RiskIQ researcher Yonathan Klijnsma identifies skimmer, BA notified |
| Sep 6, 2018 | BA publicly discloses, notifies ICO (UK regulator) |
| Jul 2019 | ICO proposes £183M fine (largest GDPR fine to-date at the time) |
| Oct 16, 2020 | Final fine: **£20M** (reduced due to COVID economic factors + BA's cooperation) |

### Numbers

- **~380,000 payment card records** stolen (full card data: number, CVV, expiry, cardholder name)
- **244,000+ frequent flyer accounts** also impacted in subsequent disclosures
- **£20M fine** (after reduction from £183M)
- **2-week window** before detection
- Class-action settlement: **undisclosed, multi-million GBP**

### Technical Root Cause

BA's payment page loaded multiple JavaScript files — first-party (BA-controlled) and third-party (analytics, A/B testing, etc.). Attackers compromised the **first-party** script (specifically a customization of `Modernizr.min.js` hosted on BA's own infrastructure) and injected ~22 lines that hooked into the payment form's submit event. On submit, the skimmer **doubled** the card data: one copy went to BA's legitimate processing (so the transaction succeeded — no user noticed), and a parallel copy was POST'd to `https://baways.com/gateway/app/dataprocessing/api/` (homoglyph attack — `baways` looks like `ba.com` in casual reading).

The skimmer used standard browser APIs only (no exotic exploit), so WAF/IPS at the server side saw nothing — the malicious data **never touched** BA's server. The exfiltration happened entirely **in the customer's browser**, to an attacker-controlled domain that had a valid TLS certificate (so even the customer's browser showed the padlock).

### Where the DFD failed

This is the **textbook DFD lesson**, listen carefully:

BA almost certainly had an architecture diagram of their payment flow. It looked something like:

```
[ Customer Browser ] ───── card data ─────►  [ BA Web Server ] ────►  [ Payment Processor ]
```

But the **actual runtime DFD**, if drawn honestly, was:

```
[ Customer Browser ] ─┬─── card data ─────►  [ BA Web Server ] ────►  [ Payment Processor ]
                      │
                      └─── card data ─────►  [ Attacker's Server (baways.com) ]    ← HIDDEN FLOW
                                                                          ▲
[ Third-party / compromised JS in browser ]  ────────────────────────────┘
```

The browser, running BA's own (compromised) JavaScript, became an **unintended data flow source**. The DFD that BA had drawn **did not represent each loaded JavaScript as a separate process inside the browser trust boundary**. Result: an entire data flow existed in production that nobody had mapped, nobody had threat-modeled, and nobody was monitoring.

### CIA + STRIDE breakdown

| Pillar | Status | How |
|--------|--------|-----|
| **C**onfidentiality | ❌ Catastrophic | 380K card records exfiltrated |
| **I**ntegrity | ⚠️ Partial | Payment data integrity to BA was intact (user transaction succeeded), but script integrity completely compromised |
| **A**vailability | ✅ Intact | Site stayed up — that's *why* it took 2 weeks to detect |

**STRIDE letters that fired:**
- **T (Tampering)** — JavaScript file on BA's own infrastructure was tampered with
- **I (Information Disclosure)** — card data exfiltrated
- **S (Spoofing)** — exfil domain `baways.com` spoofed legitimate `ba.com`

### Architectural controls that would have prevented each layer

| Layer | What failed | Architectural control |
|-------|-------------|----------------------|
| Script integrity | JS file modified without detection | **Subresource Integrity (SRI)** — `<script integrity="sha384-...">` browser refuses tampered script |
| Browser-side exfil | Skimmer could POST to any domain | **Content Security Policy (CSP)** — `connect-src 'self' payments.ba.com` blocks unknown domains |
| File-level monitoring | No alert when ba.com JS changed | **File Integrity Monitoring (FIM)** + version-pinned bundle deployment |
| DFD completeness | 3rd-party/loaded JS not modeled | **Threat model the browser as a process, with each loaded script as a code-origin-boundary-crossing** |
| Payment data path | Card data touched merchant infra | **Payment iframe/redirect (Stripe Elements pattern)** — card never enters merchant DOM |
| Detection | Exfil to unknown domain not flagged | **Egress monitoring** + DNS allowlisting for browser CSP reports |

### Architect's takeaway (quotable)

> *"Your DFD is wrong until you've drawn every loaded asset as its own process inside the customer's browser trust boundary. Browsers are not single-tenant; first-party origin ≠ first-party code."*

### Sources

- ICO Penalty Notice (Oct 16, 2020): https://ico.org.uk/media/action-weve-taken/mpns/2618421/ba-penalty-20201016.pdf
- RiskIQ Magecart analysis (Sep 11, 2018): https://www.riskiq.com/blog/labs/magecart-british-airways-breach/
- OWASP CSP Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html
- MDN Subresource Integrity: https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity

---

## 8. Pattern Recognition — T-Mobile API Breach (2023)

January 2023, T-Mobile disclosed **37 million customer accounts** exposed via an **unauthenticated API endpoint**. The endpoint returned customer profile data (name, billing address, email, phone, account number, DOB) when called with a customer ID — no auth required. Attackers iterated through IDs for ~6 weeks before detection.

**The DFD lesson:** That endpoint did not appear on any official architecture diagram. It was a legacy endpoint, possibly built for an internal system years prior, that had never been deprecated or had auth added. **An undrawn data flow = an unmonitored data flow = a 37-million-record breach.**

**Common pattern with BA Magecart:** *Assumed* data flow ≠ *actual* data flow. BA missed an outbound flow (browser → attacker server). T-Mobile missed an inbound flow (attacker → API → DB). The fix is identical: **DFDs must reflect what's running in production**, not what was originally designed.

---

## 9. Common Misconceptions Cleared

| ❌ Misconception | ✅ Reality |
|-----------------|-----------|
| "DFD = architecture diagram" | DFD focuses on data movement + trust contexts, not just components. Architecture diagrams usually skip both. |
| "Trust boundaries are network boundaries" | Network is *one* type. Process, role, code-origin, and compliance boundaries are equally important. |
| "I'll draw the DFD perfectly the first time" | First draft is always partial. Real value emerges from peer review and 2-3 revisions. |
| "DFDs are for waterfall projects" | DFDs work great in agile — short L1 sketch per feature, 15-min threat-modeling session. |
| "Tools like Threat Dragon do the thinking for me" | Tools enforce structure; *you* still have to think. Tool ≠ threat model. |
| "L0 is enough" | L0 only gives external view. Internal threats (insider, lateral movement, EoP) require L1+. |
| "Third-party SaaS doesn't go on my DFD" | It must. SaaS vendors are external entities — their compromise is your problem (Day 6 Target lesson). |

---

## 10. Glossary

- **Architecture diagram** — Generic depiction of system components and their relationships; lacks formal threat semantics.
- **C4 model** — Hierarchical software architecture notation (Context, Container, Component, Code) by Simon Brown; complements DFD.
- **Code-origin boundary** — Trust boundary between first-party code and third-party scripts loaded into the same execution context (e.g., browser).
- **Content Security Policy (CSP)** — HTTP header listing allowed origins for scripts, styles, connections, frames; browser enforces.
- **Context diagram (L0)** — Top-level DFD showing the system as a single process with its external entities.
- **Data flow** — Movement of data between elements (arrow in DFD); labeled with the data being transferred.
- **Data store** — A persistence point (DB, cache, file, queue topic); parallel-lines shape.
- **DFD (Data Flow Diagram)** — A graph showing how data moves through a system, with explicit trust boundaries; primary threat modeling tool.
- **External entity** — An actor outside the system's trust scope (user, partner, third-party service); rectangle shape.
- **File Integrity Monitoring (FIM)** — Detection technique that alerts when monitored files change unexpectedly (e.g., Tripwire, OSSEC).
- **Homoglyph attack** — Use of visually similar characters/domains to deceive users (e.g., `baways.com` vs `ba.com`).
- **Level (DFD)** — A decomposition depth: L0 (context), L1 (sub-processes), L2+ (deeper drill-down).
- **Magecart** — Umbrella term for criminal groups using JavaScript-based payment skimmers; active since 2015.
- **Process (DFD)** — A unit of transformation/computation; circle shape.
- **Subresource Integrity (SRI)** — HTML `integrity` attribute on `<script>`/`<link>` that hashes the resource; browser refuses to execute if hash mismatches.
- **Threat-modeling-as-code (TMaC)** — Defining DFDs and threats programmatically (pytm, Threagile) for versioning and CI integration.
- **Trust boundary** — Dashed line in DFD separating zones of different security context; threats concentrate at crossings.

---

## 11. Self-Check Quiz

Answer in `notes/2026-05-26-day10-data-flow-diagrams.md` **without peeking**. Mark each E/M/H + ✅/❌.

**Q1. (E)** DFD ke 5 building blocks naam le aur har ek ki shape bata.

**Q2. (E)** Trust boundary kya hai ek line mein? DFD mein woh kis shape se draw hoti hai?

**Q3. (M)** L0 vs L1 DFD ka farak — kab L0 kaafi hai aur kab L1 chahiye?

**Q4. (M)** Sequence diagram aur DFD mein 2 ka kya structural farak hai? Security ke liye DFD kyun preferred?

**Q5. (M — BA Magecart)** BA ka *assumed* DFD aur *actual* DFD mein kya farak tha? 2 architectural controls jo prevent karte?

**Q6. (M — STRIDE × DFD)** Tera "Data Flow" element pe kaunse 3 STRIDE letters apply hote hain aur baaki 3 kyun nahi?

**Q7. (H — ShopFast)** Apne L1 DFD se 3 trust-boundary crossings identify kar; har crossing pe 1 STRIDE threat + 1 mitigation.

**Q8. (H — Apne project)** System-X2 ka L0 DFD draw kar (mental). Kitne external entities? Kaunsa ek tu pehle "background mein" assume karta tha?

**Q9. (H — Futurist)** Threat-modeling-as-code (pytm/Threagile) classical whiteboard DFD ke against kya 2 advantages aur 1 trade-off rakhta hai?

---

## 12. Lab Assignment

📂 `labs/2026-05-26-day10-shopfast-dfd.md`

**Goal:** Apna pehla complete DFD set banao — ShopFast ke liye L0 + L1, trust boundaries with dashed lines, har flow labeled. Phir us DFD se 5 new threats nikalo jo Day 8/9 mein nahi mile the.

**Tools:** Excalidraw (https://excalidraw.com, free, no signup) ya draw.io. Final image PNG/SVG mein export kar ke `labs/assets/` mein commit kar.

**Time:** 45-60 min.

**Success criteria:** Lab file ke andar saari Parts (A-E) bhare hue, image embedded, kam-se-kam 5 new threats listed.

---

## 13. Reflection & Tomorrow

### Journal question
> "Apne current project (System-X2 ya koi naya feature) ka L0 + L1 DFD imagine kar. Imaandari se: kaunse 2 components ya data flows hain jo 'background mein' assume hote hain par kabhi formally draw nahi hue? Yeh saare 'undrawn flows' tera blind spot hain — woh kya hain, aur agle design review mein kis ek ko tu pehle draw karke STRIDE chalayega?"

### Connection prompt — Backend Work
> "Microservices mein har service ka apna README hota hai. Aaj se ek nayi habit propose karta hoon: har service README ke top pe ek small ASCII DFD ho — 'kaun mujhe call karta hai, main kaun ko call karta hoon, mera data kahan rukta hai, trust boundaries kahan hain.' 5 minute ka kaam, lifetime ka clarity. Ek apne service pe try karke dekh."

### Tomorrow's Preview
**Day 11 → Trust Boundaries: jahan threats paida hote hain.** Aaj DFD mein humne dashed lines draw karna seekha. Kal sirf un dashed lines pe deep-dive — kaun-kaun se trust boundaries hoti hain (network, process, privilege, org, code-origin), aur har crossing pe kya questions poochne hain. Zero-trust architecture ka conceptual foundation.

---

## 14. Progress Entry

```
Day 010 | 2026-05-26 | Q1 | Data Flow Diagrams (DFDs) — your first one | ✅ Done | DFD = STRIDE ka canvas; 5 building blocks + trust boundary; BA Magecart 2018 = assumed-flow ≠ actual-flow ka textbook case (missing browser-side JS process on DFD); ShopFast L0+L1 drawn
```

---

#dfd #data-flow-diagram #threat-modeling #trust-boundary #shostack-q1 #stride-per-element #british-airways-magecart-2018 #subresource-integrity #content-security-policy #t-mobile-api-breach-2023 #shopfast #l0-context-diagram #l1-decomposition #threat-modeling-as-code #pytm #threagile #futurist-tmac

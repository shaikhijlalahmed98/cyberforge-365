# 🔬 Labs

Hands-on work. This is where theory becomes muscle memory.

## Structure

```
labs/
├── README.md              ← You are here
├── day-XXX/               ← One folder per lab day
│   ├── README.md          ← What I built, how, why
│   ├── code/              ← Source files
│   ├── configs/           ← Terraform, K8s manifests, IAM policies
│   ├── screenshots/       ← Evidence of working exploits/defenses
│   └── findings.md        ← Pentest-style report when applicable
└── shopfast/              ← The persistent fictional system I'm protecting
    ├── architecture.md
    ├── threat-model.md
    └── ...
```

## The ShopFast Project

Throughout the 365 days, I'm playing security architect for **ShopFast** — a fictional e-commerce platform. Every monthly capstone builds on it:

- **Month 1:** Initial threat model
- **Month 2:** Crypto architecture
- **Month 3:** IAM redesign
- **Month 4:** Zero-trust network
- **Month 5:** Secure API design
- **Month 6:** Cloud security reference architecture
- **Month 7:** Enterprise threat model v2
- **Month 8:** Detection & response playbook
- **Month 9:** Red team exercise findings
- **Month 10-12:** Specialization deep dive

By Day 365, `labs/shopfast/` is a **complete reference architecture** I can show in interviews.

## Lab quality bar

Each lab folder should have:
- A clear `README.md` explaining what & why
- Reproducible setup (Docker Compose, Terraform, Ansible)
- Evidence — screenshots, logs, test output
- A "what I learned" section

Half-finished labs are okay. Skipped labs aren't. **Always commit, even if it's broken** — the progression matters.

## Tools by phase

**Q1:** OpenSSL, OWASP Threat Dragon, JWT.io, Auth0/Keycloak
**Q2:** AWS Free Tier, Burp Suite Community, OWASP ZAP, Trivy, Semgrep
**Q3:** Elastic Stack, Wireshark, Volatility, Atomic Red Team
**Q4:** Specialization-specific

## Sandbox safety

All exploitation is done on:
- Personal machines / isolated VMs
- Deliberately vulnerable apps (WebGoat, DVWA, HackTheBox)
- AWS Free Tier with strict cost alerts
- **Never on production or third-party systems I don't own**

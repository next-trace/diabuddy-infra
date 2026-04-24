# Privacy Policy — DRAFT (NOT LEGALLY REVIEWED)

> **This document is a structural template.** It MUST be reviewed by an EU privacy lawyer before being presented to any real user. Do not publish as-is.

_Last updated: PLACEHOLDER_DATE_

## 1. Data controller

**Name:** PLACEHOLDER_LEGAL_ENTITY
**Address:** PLACEHOLDER_EU_ADDRESS
**Contact:** privacy@PLACEHOLDER_DOMAIN
**Data Protection Officer (DPO):** PLACEHOLDER_DPO_NAME — dpo@PLACEHOLDER_DOMAIN

## 2. What we collect

| Category | Examples | Legal basis |
|---|---|---|
| Account identifiers | Email, name, password hash, account creation timestamp | GDPR Art. 6(1)(b) — contract performance |
| **Health data (Art. 9)** | Blood glucose readings, insulin doses, carbohydrate intake, meal photos, symptoms, device pairings | **GDPR Art. 9(2)(a) — explicit consent** |
| Usage data | Login timestamps, feature usage, device/browser type | GDPR Art. 6(1)(f) — legitimate interest |
| Diagnostic / crash data | Stack traces (no PII), request latencies | GDPR Art. 6(1)(f) — legitimate interest |

We do not knowingly collect data about children under 16. Our service is not directed to minors.

## 3. Why we process it

| Purpose | Data categories used |
|---|---|
| Provide the core service (you track your glucose/insulin/meals) | Account identifiers + health data |
| Authenticate you | Account identifiers |
| Respond to your clinician's summary request (if you opt in) | Health data |
| Improve product stability | Diagnostic data |
| Comply with legal obligations (tax, anti-fraud) | Account identifiers |

**We do not use your health data for advertising, for training third-party AI models, or for sale to data brokers.** Full stop.

## 4. Who we share it with (processors)

| Processor | Role | Location | DPA signed |
|---|---|---|---|
| **DigitalOcean LLC** | Hosting (compute, storage, networking) | EU (Frankfurt/Amsterdam) | YES — see DO DPA at digitalocean.com/legal/data-processing-agreement |
| **Better Stack** (optional, if logging sink enabled) | Log aggregation | EU | PLACEHOLDER_STATUS |
| **OpenAI / Google / Anthropic** (optional, only if AI features enabled and you opt in separately) | LLM inference on anonymized meal descriptions | US | PLACEHOLDER_STATUS |

We do not transfer your health data outside the EU/EEA by default. AI features that rely on US processors are **opt-in per feature** with a separate consent flow.

## 5. How long we keep it

| Category | Retention |
|---|---|
| Account identifiers | 7 years after account closure (or as required by applicable tax/law) |
| Health data | 90 days after account closure unless you request earlier deletion; clinician summaries retained 10 years per EU medical record retention guidance (REVIEW WITH DPA LAWYER) |
| Logs | 30 days |
| Backups | 30 days, encrypted at rest |

You can request erasure at any time (see §8).

## 6. Where we store it

Primary: DigitalOcean EU region (Frankfurt or Amsterdam droplet). Backups: encrypted Postgres dumps in DigitalOcean Spaces (same region, encrypted with a key we control).

No transfer to non-EU processors for your health data unless you explicitly opt into a feature that requires one.

## 7. Your rights (GDPR Art. 12–22)

- **Access** (Art. 15) — request a copy of everything we hold on you
- **Rectification** (Art. 16) — fix inaccuracies
- **Erasure** (Art. 17) — delete your account and all associated data
- **Restriction** (Art. 18) — pause processing
- **Portability** (Art. 20) — export your data in a machine-readable format (JSON)
- **Objection** (Art. 21) — stop processing based on legitimate interest
- **Withdraw consent** (Art. 9(2)(a)) — withdraw consent for health-data processing at any time; we will stop and delete the relevant data within 30 days
- **Complain** to a supervisory authority (typically in your EU country of residence)

Exercise any right: privacy@PLACEHOLDER_DOMAIN or the in-app "Export my data" / "Delete my account" buttons.

## 8. How to delete or export

- **In-app**: Settings → Data & Privacy → "Delete my account" (irreversible) or "Export my data" (JSON).
- **By email**: privacy@PLACEHOLDER_DOMAIN — response within 30 days.

## 9. Security

See `SECURITY.md` in the public deployment repo for technical details. Summary:
- TLS 1.2+ for all traffic
- Passwords bcrypt-hashed (cost factor 12)
- Secrets never in source code
- Encrypted backups
- UFW firewall + fail2ban on servers
- Role-based access control for any internal operator

In the event of a breach, we notify the supervisory authority within 72 hours per GDPR Art. 33 and notify you directly if the risk is high (Art. 34). See BREACH-PLAYBOOK.md.

## 10. Cookies + tracking

The web app uses strictly-necessary cookies only (session, CSRF). No analytics cookies without explicit opt-in. No third-party tracking pixels.

## 11. Changes to this policy

We notify registered users by email 30 days before any material change takes effect.

## 12. Governing law

This policy is governed by the law of PLACEHOLDER_COUNTRY and subject to the exclusive jurisdiction of the courts of PLACEHOLDER_CITY.

---

_— END OF DRAFT —_

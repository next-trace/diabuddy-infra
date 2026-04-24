# Breach response playbook

> GDPR Art. 33: supervisory authority must be notified within **72 hours** of awareness, except where unlikely to result in risk to rights/freedoms. Art. 34: data subjects must be notified "without undue delay" when risk is high.

## Phase 0 — Detect (0–30 min)

Triggers:
- Automated alert (failed healthcheck, unusual auth failure spike, unexpected DB growth, SSL cert anomaly)
- User report (security@ email, support ticket mentioning account takeover)
- Third-party notification (DO, Better Stack, dep vulnerability advisory)
- Internal observation (operator notices anomaly)

**First responder actions (T+0 to T+30m):**
- Acknowledge the alert in the team channel with the tag `#security-incident`
- Open a private incident doc (Google Doc or similar) — title format `INC-YYYY-MM-DD-short-desc`
- Start the timeline. Every subsequent action gets a UTC timestamp in that doc.
- Preserve evidence: snapshot logs, take a disk image if on a self-hosted droplet, don't reboot unless containment requires it.

## Phase 1 — Contain (30 min – 4 h)

Goals:
1. Stop the bleeding (revoke credentials, isolate host, pull compromised images)
2. Prevent further exfiltration (rotate secrets, kill active sessions)
3. Do NOT destroy evidence (no `rm -rf` on log files, no wiping the droplet)

Common containment actions:
- Rotate `JWT_SECRET` + force logout all users (side effect: users re-login)
- Rotate `POSTGRES_PASSWORD` (coordinate: update `.env`, redeploy, brief downtime)
- Rotate API keys to all processors (OpenAI, Anthropic, Better Stack)
- Block suspicious IP via UFW if source identified
- If droplet compromise suspected: take snapshot, then isolate from internet (UFW deny all inbound except 22 from a single admin IP) until forensics done

## Phase 2 — Assess (4 h – 24 h)

Questions to answer (populate in the incident doc):
1. **What happened?** Root cause, attack vector, timeline
2. **What data was affected?** Categories (Art. 9 health data vs only account identifiers), approximate number of data subjects, geographic scope
3. **What was the likely impact?** Confidentiality / integrity / availability breach — one, two, or all three?
4. **Is it ongoing or contained?**
5. **Are we obligated to notify?**
   - Art. 33 notification to supervisory authority: YES unless "unlikely to result in risk to rights/freedoms"
   - Art. 34 notification to data subjects: YES if "likely to result in high risk"
   - Health data almost always crosses the high-risk threshold — **default is notify**
6. **Do we have cyber insurance?** If yes, notify carrier BEFORE making public statements.

## Phase 3 — Notify (within 72 h)

### 3.1 Supervisory authority (GDPR Art. 33) — REQUIRED within 72h

Lead supervisory authority (LSA): whichever SA covers the controller's main establishment. For a EU-established controller, usually the SA of the country of registered office.

**Template notification content (Art. 33(3)):**
> - Nature of the breach + categories of data + approximate number of data subjects affected
> - DPO contact (name, email, phone)
> - Likely consequences
> - Measures taken / proposed to address the breach and mitigate impact

Most SAs have an online breach-notification form. Know yours in advance. Submit even if you don't have all details — you can supplement later (Art. 33(4)).

### 3.2 Data subjects (GDPR Art. 34) — when risk is HIGH

Required if breach "is likely to result in a high risk to rights and freedoms" — nearly always the case for health data breaches.

Must be in **clear plain language**. Content (Art. 34(2)):
> - Description of the breach
> - DPO contact
> - Likely consequences
> - Measures taken or proposed

Delivery: email to affected users + in-app banner. Do NOT delay to get it polished — speed matters more than perfection here.

Exceptions (Art. 34(3)) — may skip direct notification IF:
- Data was encrypted at rest with a key the attacker did not obtain, OR
- Subsequent measures ensure high risk is no longer likely, OR
- Direct communication would involve disproportionate effort (then public communication instead)

### 3.3 Processors (contractual)

- DO — if a DO-side incident, they have obligations to us under the DPA
- We have obligations to anyone whose data we process as a processor (N/A currently)

## Phase 4 — Eradicate (24 h – 7 d)

- Patch the vulnerability that caused the breach
- Re-image the compromised host if OS-level compromise suspected
- Restore from a clean backup if data integrity compromised
- Rotate all remaining secrets (even non-obviously-compromised)
- Audit all accounts created or modified during the incident window

## Phase 5 — Recover (1 d – 14 d)

- Bring systems back online gradually
- Monitor for recurrence (heightened alerting for 30 days)
- Validate data integrity (sample checks)
- Re-enable any features that were disabled for containment

## Phase 6 — Post-incident (14 d – 30 d)

- Post-mortem document: what happened, timeline, root cause, remediation, lessons learned, action items
- Update this playbook if the incident exposed a gap
- Review insurance claim if applicable
- Close the supervisory authority case with a final report

## Contacts (fill in before launch)

| Role | Name | Email | Phone |
|---|---|---|---|
| DPO | PLACEHOLDER | dpo@PLACEHOLDER | PLACEHOLDER |
| Legal | PLACEHOLDER | PLACEHOLDER | PLACEHOLDER |
| Cyber insurance | PLACEHOLDER | PLACEHOLDER | PLACEHOLDER |
| PR lead | PLACEHOLDER | PLACEHOLDER | PLACEHOLDER |
| Lead SA | PLACEHOLDER | PLACEHOLDER portal URL | PLACEHOLDER |
| DO support | — | support@digitalocean.com | (via DO ticket) |

## Templates

- `templates/art33-notification.md` — SA notification template (TODO)
- `templates/art34-user-email.md` — user notification email (TODO)
- `templates/post-mortem.md` — post-mortem structure (TODO)

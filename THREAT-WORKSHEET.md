# Completed threat worksheet — AMPS practice

Completed October 19, 2026. Fictional learning system; proposed requirements and tests, not verified product controls. Model records 41–43 correspond to T-01–T-03 below. All three are **Needs Investigation**. No security tests were executed.

## Scope and assumptions

Protect integrity, confidentiality and timely delivery of readings across wearable → phone → cloud, storage, and clinician review. The phone is a potentially compromised relay. A valid user token does not prove ownership of a supplied device ID. No remote therapy commands are in scope. Incorrect, stale or unavailable readings might affect clinical review; clinical severity and acceptable delay are not established.

Omitted dependencies: identity provider, clinician portal/browser, device enrollment and key provisioning, firmware update/signing services, manufacturing/service interfaces, backups and detailed mobile/device storage. Each requires a later model iteration. The label “HTTPS” describes intended transport, not tested TLS configuration. Likewise “validated” and “authorized” are intended outcomes, not evidence. Template default No/Not Selected properties are unassessed placeholders, not discovered vulnerabilities.

## Step 3 — STRIDE review of phone → API

| Category | Attacker/precondition | Threat and possible consequence | Proposed response |
| --- | --- | --- | --- |
| Spoofing | Stolen user credential, substituted device identity, or compromised relay | Readings attributed to the wrong origin | Validate token trust/lifetime/audience and device enrollment; establish whether device-origin signatures are required |
| Tampering | Authorized relay can alter payload or identifiers | False, reassigned or stale readings influence review | Per-object authorization, schema validation and authenticated provenance/freshness; TLS alone cannot constrain a compromised sender |
| Repudiation | Inadequate event records or correlatable identities | Cannot reconstruct who submitted a disputed reading | Protected audit events with caller, device, outcome, server time, measurement reference and correlation ID; no secrets or unnecessary PHI |
| Information disclosure | Broken patient-object authorization or excessive logging | Unauthorized patient access or leakage | Per-patient read/write policy, response minimization, protected logs and transport |
| Denial of service | Reachable endpoint; expensive/unbounded processing | Upload/review delays or missing data | Work limits, quotas, backpressure, recovery and clinically appropriate missing-data visibility |
| Elevation of privilege | Patient role accepted on clinician/admin operations | Unauthorized privileged actions | Server-side function/role enforcement plus object checks; negative role matrix tests |

## Step 4 — Three detailed threat/control/test chains

### T-01 — Cross-patient upload (model ID 41)

**Flow:** phone → API. **STRIDE:** tampering / elevation of privilege. **Precondition:** authenticated patient A can submit caller-controlled identifiers, and backend relationship enforcement is absent or incorrect. **Consequence:** B's record may contain A's or fabricated readings.

**REQ-01:** authorize every upload against the server-controlled relationship among caller, enrolled device and patient. Deny unknown/unbound associations. Validate token trust properties separately. Device enrollment/reassignment must be authorized and auditable.

**TEST-01:** create synthetic patients/devices A and B. Upload with A's valid token and B's device ID; repeat with B's patient ID, mixed fields, unknown IDs, bulk requests and an expired enrollment. Expect denial with zero changes to B's data and a sanitized denial event. Positive control: A uploading to A succeeds. Also test read endpoints and role variations.

**Owner roles:** API engineering implements; security validates; identity/device engineering owns enrollment policy. **Evidence needed:** build/config revision, synthetic fixture description, request/response records, before/after data checks and audit event. **Residual questions:** reassignment, caregiver access, compromised enrollment authority and provenance of readings. **Status:** Needs Investigation; no implementation/test evidence. High is a provisional exercise priority, not a clinical severity rating.

### T-02 — Replay/stale readings (model ID 42)

**Flow:** phone → API, with end-to-end dependency on wearable provenance. **STRIDE:** tampering. **Precondition:** captured valid payload or compromised authorized relay; inadequate uniqueness/freshness enforcement. **Consequence:** stale or duplicate readings may appear current.

**REQ-02:** define authenticated measurement identity/sequence/time, deduplication and measurement-versus-receipt timestamps. Specify offline synchronization, clock drift/reset, reboot and key-rotation behavior. Do not trust the phone's clock as proof of measurement age. The acceptable freshness interval requires product/clinical agreement.

**TEST-02:** replay an identical accepted upload, modify an integrity-protected sequence/time, reorder messages, replay across sessions, and submit legitimate delayed offline data. Expect duplicate rejection or idempotent processing, tamper detection where provenance is protected, and no stale reading relabeled fresh. Valid offline readings retain their age and follow the agreed policy. Test reset/rotation boundaries and recovery.

**Owner roles:** device/mobile/API engineering; clinical/product owner defines freshness behavior. **Evidence needed:** packet/fixture identities, sequence/timestamp rules, stored results, UI age indication and negative/positive test results. **Residual questions:** compromised measurement source, trustworthy time and reset-resistant counters. **Status:** Needs Investigation; no tests run; provisional High priority.

### T-03 — Resource exhaustion (model ID 43)

**Flow:** phone → API → database; may affect clinician access. **STRIDE:** denial of service. **Precondition:** reachable endpoint and unbounded expensive work, attacker identities or compromised valid client. **Consequence:** delayed/missing uploads and review.

**REQ-03:** bound payload size, parsing work, concurrency, execution time and queues; apply context-aware quotas/backpressure and safe retries; expose missing-data/recovery state. Define legitimate workloads and availability budgets before setting limits. Gateway controls complement backend/database controls.

**TEST-03:** in an isolated test environment, mix representative legitimate traffic with oversized bodies, bursts, expensive payloads and retries. Expect abusive work to remain bounded while normal traffic meets approved latency/error budgets. Confirm alerts, recovery time, backlog handling and no duplicate or silently dropped accepted readings. Test that quotas do not block legitimate offline synchronization.

**Owner roles:** API/SRE and database engineering; clinical/product partners establish tolerable delay. **Evidence needed:** exact workload/limits, resource and latency measurements, rejection/queue metrics, recovery timeline and data reconciliation. Numerical thresholds are TBD rather than invented. **Residual questions:** distributed abuse, shared-dependency failures and field connectivity. **Status:** Needs Investigation; no load test executed; provisional High priority.

## Closure and residual-risk decision

A proposed control is insufficient to mark Mitigated. Closure requires implementation/configuration traceability, meaningful passing negative and positive tests, evidence review and a documented residual-risk decision by the organization's designated authority. Product/clinical safety and quality/regulatory partners assess clinical implications; a security engineer cannot assign clinical severity alone. Unacceptable residual risk requires redesign or additional controls and retesting.

## What the exercise demonstrated

We completed modeling, structured threat identification, three requirement/control/test chains and a comparison with actual tool output. We did not implement controls, validate clinical behavior or complete a regulatory submission. See EXERCISE-REVIEW.md for tool comparison and all 41 generated findings.

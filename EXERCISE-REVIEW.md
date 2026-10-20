# Exercise review and learning debrief

Completed October 19, 2026. This is a fictional design study, not an assessment of a real product.

## Step 5 — Actual tool comparison

Microsoft Threat Modeling Tool Analysis view generated **41 findings** from the community medical-device template. The unchanged baseline export is AMPS-Generated-Threats.csv. Three explicit scenario records were added to the model: ID 41 / T-01 authorization, ID 42 / T-02 replay, and ID 43 / T-03 upload exhaustion. All three are Needs Investigation with proposed controls, tests, and owner roles; none are Mitigated.

The tool's initial findings were all High and Not Started. This is a template result, not a prioritized clinical risk assessment. A generated prompt does not prove a vulnerability, and absence of a generated prompt does not establish security.

| Category | Generated findings |
| --- | --- |
| Denial Of Service | 8 |
| Elevation Of Privilege | 12 |
| Information Disclosure | 4 |
| Repudiation | 4 |
| Spoofing | 9 |
| Tampering | 4 |

### What required human reasoning

- **T-01:** the generated list has spoofing/input-validation/access-control prompts, but does not explicitly describe cross-patient upload with a valid token and another device's ID. We added the concrete business-authorization scenario.
- **T-02:** no baseline title explicitly describes replay/freshness or legitimate delayed offline uploads. We added both malicious and legitimate cases; sequence/time/provenance need design decisions.
- **T-03:** IDs 3, 24 and 25 overlap with resource exhaustion, crashes and interruption. Our added scenario makes the requirement/test/evidence chain specific to uploads and downstream clinical review.
- **Generated ID 18, CSRF on BLE:** provisionally not applicable under the stated assumption of a native BLE input path with no browser/cookie/webview mechanism. This is an assumption-dependent disposition; it does not dismiss CSRF across the whole product. The tool record remains unclosed until implementation confirms the assumption.
- **IDs 28 and 40, API/clinician CSRF:** unresolved because the browser/session/authentication design is omitted. Bearer-only native calls and cookie-authenticated browser calls have different exposure.

### Model-quality limitations

The diagram uses generic processes/flows. Names like “HTTPS,” “authorized,” and “validated” do not configure or prove security properties. Many template attributes remain No or Not Selected defaults. Treat these as unknowns to resolve with engineering, not factual findings. Add more specific stencils/properties only when implementation supports the choice; never suppress findings simply to reduce the count.

The identity provider, enrollment flow, portal, key lifecycle, firmware updates and detailed device/mobile storage are omitted. This exercise is therefore incomplete as a product-wide risk assessment. Those omissions are a next iteration, not grounds for accepting their risks.

## Per-finding review ledger

This ledger reviews every generated finding. Generated tool states remain Not Started; the ledger records first-pass triage. No generated finding was declared mitigated. IDs are the tool's baseline IDs.

| Tool ID | Generated title | First-pass review / rationale |
| --- | --- | --- |
| 0 | Elevation Using Impersonation | Needs investigation: identify actual privileges and delegated execution context; generic impersonation wording is not evidence of a vulnerability. |
| 1 | Elevation Using Impersonation | Needs investigation: identify actual privileges and delegated execution context; generic impersonation wording is not evidence of a vulnerability. |
| 2 | Spoofing of Destination Data Store Patient readings database | Needs investigation: verify database endpoint identity, authenticated connections, certificate validation and workload permissions. |
| 3 | Potential Excessive Resource Consumption for Cloud ingestion API or Patient readings database | Needs investigation: compare T-03; analyze resource/connection failures, missing-data visibility, recovery, and clinically acceptable latency. |
| 4 | Spoofing of Source Data Store Patient readings database | Needs investigation: verify database endpoint identity, authenticated connections, certificate validation and workload permissions. |
| 5 | PHI at Rest Tampering | Needs investigation: evaluate stored PHI integrity, write permissions, audit trails and restoration. Clinical severity is unresolved. |
| 6 | Weak Access Control for a Resource | Needs investigation: enforce patient-level read policy and least-privilege workload access; compare T-01, including retrieval tests. |
| 7 | Spoofing the Clinician External Entity | Needs investigation: identify enrollment/token/certificate trust and credential lifecycle; human, device and workload identities are distinct. |
| 8 | Elevation Using Impersonation | Needs investigation: identify actual privileges and delegated execution context; generic impersonation wording is not evidence of a vulnerability. |
| 9 | Spoofing the Wearable measurement service Process | Needs investigation: identify enrollment/token/certificate trust and credential lifecycle; human, device and workload identities are distinct. |
| 10 | Spoofing the Patient mobile app Process | Needs investigation: identify enrollment/token/certificate trust and credential lifecycle; human, device and workload identities are distinct. |
| 11 | Potential Lack of Input Validation for Patient mobile app | Needs investigation: validate structure, bounds and semantics. T-01 object authorization and T-02 freshness require additional explicit requirements. |
| 12 | Potential Data Repudiation by Patient mobile app | Needs investigation: define protected audit events, server times, identity/correlation and PHI minimization; logging alone does not provide absolute nonrepudiation. |
| 13 | Data Flow Sniffing | Needs investigation: inspect actual transport and peer validation. A flow named HTTPS is not proof of secure TLS; template defaults are not measured results. |
| 14 | Potential Process Crash or Stop for Patient mobile app | Needs investigation: compare T-03; analyze resource/connection failures, missing-data visibility, recovery, and clinically acceptable latency. |
| 15 | Data Flow BLE patient readings Is Potentially Interrupted | Needs investigation: compare T-03; analyze resource/connection failures, missing-data visibility, recovery, and clinically acceptable latency. |
| 16 | Patient mobile app May be Subject to Elevation of Privilege Using Remote Code Execution | Needs investigation: review parser/input exposure, runtime isolation, dependency risk and least privilege; this diagram does not establish an exploit. |
| 17 | Elevation by Changing the Execution Flow in Patient mobile app | Needs investigation: review parser/input exposure, runtime isolation, dependency risk and least privilege; this diagram does not establish an exploit. |
| 18 | Cross Site Request Forgery | Provisionally not applicable to this BLE-only interaction: assume native GATT input with no browser, cookies or webview; confirm implementation before closing. CSRF is revisited for web/API flows. |
| 19 | Spoofing the Patient mobile app Process | Needs investigation: identify enrollment/token/certificate trust and credential lifecycle; human, device and workload identities are distinct. |
| 20 | Spoofing the Cloud ingestion API Process | Needs investigation: identify enrollment/token/certificate trust and credential lifecycle; human, device and workload identities are distinct. |
| 21 | Potential Lack of Input Validation for Cloud ingestion API | Needs investigation: validate structure, bounds and semantics. T-01 object authorization and T-02 freshness require additional explicit requirements. |
| 22 | Potential Data Repudiation by Cloud ingestion API | Needs investigation: define protected audit events, server times, identity/correlation and PHI minimization; logging alone does not provide absolute nonrepudiation. |
| 23 | Data Flow Sniffing | Needs investigation: inspect actual transport and peer validation. A flow named HTTPS is not proof of secure TLS; template defaults are not measured results. |
| 24 | Potential Process Crash or Stop for Cloud ingestion API | Needs investigation: compare T-03; analyze resource/connection failures, missing-data visibility, recovery, and clinically acceptable latency. |
| 25 | Data Flow HTTPS upload: readings + device identity Is Potentially Interrupted | Needs investigation: compare T-03; analyze resource/connection failures, missing-data visibility, recovery, and clinically acceptable latency. |
| 26 | Cloud ingestion API May be Subject to Elevation of Privilege Using Remote Code Execution | Needs investigation: review parser/input exposure, runtime isolation, dependency risk and least privilege; this diagram does not establish an exploit. |
| 27 | Elevation by Changing the Execution Flow in Cloud ingestion API | Needs investigation: review parser/input exposure, runtime isolation, dependency risk and least privilege; this diagram does not establish an exploit. |
| 28 | Cross Site Request Forgery | Conditional: determine whether browser cookies/ambient credentials and a state-changing operation are present. Do not close without portal/authentication details. |
| 29 | Spoofing of the Clinician External Destination Entity | Needs investigation: identify enrollment/token/certificate trust and credential lifecycle; human, device and workload identities are distinct. |
| 30 | External Entity Clinician Potentially Denies Receiving Data | Needs investigation: define protected audit events, server times, identity/correlation and PHI minimization; logging alone does not provide absolute nonrepudiation. |
| 31 | Data Flow Authorized results Is Potentially Interrupted | Needs investigation: compare T-03; analyze resource/connection failures, missing-data visibility, recovery, and clinically acceptable latency. |
| 32 | Spoofing the Cloud ingestion API Process | Needs investigation: identify enrollment/token/certificate trust and credential lifecycle; human, device and workload identities are distinct. |
| 33 | Potential Lack of Input Validation for Cloud ingestion API | Needs investigation: validate structure, bounds and semantics. T-01 object authorization and T-02 freshness require additional explicit requirements. |
| 34 | Potential Data Repudiation by Cloud ingestion API | Needs investigation: define protected audit events, server times, identity/correlation and PHI minimization; logging alone does not provide absolute nonrepudiation. |
| 35 | Data Flow Sniffing | Needs investigation: inspect actual transport and peer validation. A flow named HTTPS is not proof of secure TLS; template defaults are not measured results. |
| 36 | Potential Process Crash or Stop for Cloud ingestion API | Needs investigation: compare T-03; analyze resource/connection failures, missing-data visibility, recovery, and clinically acceptable latency. |
| 37 | Data Flow Authenticated readings request Is Potentially Interrupted | Needs investigation: compare T-03; analyze resource/connection failures, missing-data visibility, recovery, and clinically acceptable latency. |
| 38 | Cloud ingestion API May be Subject to Elevation of Privilege Using Remote Code Execution | Needs investigation: review parser/input exposure, runtime isolation, dependency risk and least privilege; this diagram does not establish an exploit. |
| 39 | Elevation by Changing the Execution Flow in Cloud ingestion API | Needs investigation: review parser/input exposure, runtime isolation, dependency risk and least privilege; this diagram does not establish an exploit. |
| 40 | Cross Site Request Forgery | Conditional: determine whether browser cookies/ambient credentials and a state-changing operation are present. Do not close without portal/authentication details. |

## Acceptance criteria and evidence review

| Exercise task | Result |
| --- | --- |
| Define scope and assumptions | Completed in THREAT-WORKSHEET.md |
| Draw system and boundaries | Saved AMPS-Practice.tm7; five components, six directed flows, three trust zones |
| Apply STRIDE before relying on generated titles | Six-category reasoning documented; we had already discussed the three scenarios before opening Analysis view |
| Detail three controls and verification plans | T-01–T-03 in worksheet and model IDs 41–43 |
| Compare with real tool findings | 41 baseline rows reviewed in this ledger |
| Verify protections in software/hardware | Not performed; no implementation exists in this exercise |
| Decide clinical severity and accept residual risk | Not performed; requires product facts and designated clinical/quality/risk owners |

Before calling a control effective: identify the tested build/configuration, document fixtures and expected outcomes, run negative and legitimate-use tests, inspect actual stored results and logs, retain evidence, and obtain the designated review. Review the model when interfaces, dependencies, intended use, identity policy or threat knowledge changes.

## Learning debrief

“I built a fictional wearable/mobile/cloud model, mapped trust boundaries, applied STRIDE, and reviewed 41 template-generated findings. I added explicit cross-patient authorization and replay scenarios that the generic titles did not fully capture. For three threats I wrote security requirements, owner roles and negative/positive verification criteria. I kept them open because I had not implemented or tested the controls. I also documented assumptions and deferred clinical severity to qualified partners.”

### Review questions

- **Why isn't TLS sufficient?** An authenticated compromised caller can still send another patient's device ID or replay data. Backend authorization and provenance/freshness are separate requirements.
- **How do you prove an authorization fix?** A/B identity-object matrix, denied requests, unchanged unauthorized records, legitimate positive cases and audit evidence.
- **What would you change first?** Resolve identity/enrollment assumptions and clinically acceptable freshness/availability; then implement and verify the three threat chains in the team's risk order.
- **Why not close everything marked High?** Priority came from generic template output. Confirm applicability and consequences before assessing risk; proposed mitigations do not justify closure.
- **What is your experience boundary?** This is independent learning. It demonstrates a repeatable method, not FDA-submission ownership or production medical-device engineering.

## Reference comparison

The linked MITRE/MDIC playbook uses fictional AMPS diagrams, trust boundaries and a STRIDE walkthrough (sections 2.1, 2.3 and 2.4.2). Our first-pass model follows those concepts but intentionally simplifies the system, omits some dependencies, and adds a specific authorization/freshness exercise. Do not present it as the playbook's complete AMPS model.

- [MITRE/MDIC playbook](https://mdic.org/wp-content/uploads/2021/11/Playbook-for-Threat-Modeling-Medical-Devices.pdf)
- [FDA February 2026 guidance — consult original for regulatory expectations](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/cybersecurity-medical-devices-quality-management-system-considerations-and-content-premarket)
- [MITRE EMB3D — optional next iteration for embedded-device threats](https://emb3d.mitre.org/)

## Next iteration, after this exercise

Add enrollment/identity-provider flows; model firmware signing/update and recovery; identify device key storage and renewal; represent offline operation with a state diagram. Reassess threats and add implementation-specific tests. These are future extensions, not silently assumed controls in the completed first-pass exercise.



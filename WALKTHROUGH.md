# Reproduce the exercise

Allow approximately 45 minutes for an initial pass, then additional time for detailed review. The system is fictional and inspired by the simplified AMPS study in the MITRE/MDIC playbook.

1. **Define scope.** Protect integrity, confidentiality and timely delivery of readings. The phone may be compromised; therapy commands are excluded. Record the omitted dependencies listed in the README.
2. **Draw the five elements and six directed flows.** Use the README diagram as a guide. Draw separate wearable, phone and cloud trust boundaries. The clinician sits outside the cloud boundary.
3. **Analyze the app-to-API flow manually.** For each STRIDE category, specify an attacker, preconditions and consequence. Ask whether the API trusts caller-supplied patient/device identifiers and whether old readings can appear current.
4. **Define three concrete threat chains.** For authorization, replay/freshness and resource exhaustion, document requirement, proposed control, positive/negative tests, expected result, owner roles and remaining uncertainty. Compare with the worksheet after your attempt.
5. **Compare automated output.** Open Analysis view. Generated findings are investigation prompts. Explain overlaps, omissions and applicability. The baseline in this repository contains 41 findings; different properties or template/tool versions may produce different output.
6. **Review every finding.** Use the ledger to see why a BLE CSRF prompt is only provisionally inapplicable under explicit assumptions, while browser-related CSRF remains unresolved.
7. **Preserve honest states.** Proposed controls do not justify marking a finding mitigated. The saved model retains 41 baseline records and adds three `NeedsInvestigation` scenarios. The written ledger and original generated states are distinct artifacts.
8. **Debrief.** Explain one chain in 60 seconds: boundary → attacker/precondition → consequence → control/requirement → test → uncertainty.

No implementation tests were executed in this exercise. Define clinically appropriate freshness and availability thresholds with qualified partners rather than inventing them.

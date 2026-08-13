Hi Baba-san, thank you. I think I understand the purpose of the Vulnerability Investigation Results folders now, but I would like to confirm the expected responsibility and workflow before I begin using them.

My understanding is that when FossID generates a new vulnerability notification, we should:

1. Review the notification and identify the affected product/SBOM, component, version, CVE, and severity.
2. Confirm the component/version detected by Syft/FossID and review the available vulnerability information.
3. Ask the responsible Engineering team to determine where the component is located, what application/runtime requires it, whether it is included in the deployed/customer environment, and whether the vulnerable version is actually applicable.
4. If applicable, Engineering determines whether it can be removed, upgraded, recompiled, or otherwise remediated. If immediate remediation is not feasible, Engineering provides the technical justification, mitigation, and remediation plan.
5. Document the investigation and final disposition in the Vulnerability Investigation Results record for traceability and EU CRA evidence.

For the current multiple-version investigation, Engineering has confirmed that at least some of the zlib and log4j versions are dependencies of MATLAB runtimes, and they are using the Syft JSON paths to determine which applications contain them. They also confirmed that the program containing the vulnerable Go stdlib can be removed. I am following up with them to classify the remaining versions more clearly.

Could you please confirm who is expected to own and complete the Vulnerability Investigation Results document? Should TEH Security conduct and document the investigation, should PSS/PSIRT in Japan own it, or should the responsible Engineering team complete the technical investigation with Security/PSIRT reviewing the result?

My proposed workflow would be for Security/PSIRT to own vulnerability triage and tracking, while Engineering owns the technical applicability assessment and remediation decision because they understand the product/runtime dependencies. Security/PSIRT would then review the Engineering response and ensure the investigation result and evidence are documented before the finding is considered complete.

If this matches the intended operation, I would like to establish it as a repeatable workflow with Engineering so that future FossID vulnerability notifications have clear ownership, assessment requirements, remediation/justification, and documentation rather than handling each notification ad hoc.







Thanks Kurt, the information about the MATLAB runtimes helps clarify why we’re seeing multiple versions.

For the current investigation, and to help us establish a repeatable vulnerability-handling process going forward, I think we need to classify the critical/high-priority components detected by Syft a little more specifically.

For each vulnerable component/version, could Engineering help us identify:

* The component and version
* The path where Syft detected it
* The application/runtime that requires or contains it
* Whether it is included in the deployed/customer product image
* Whether it is actually required at runtime
* Whether the vulnerability is applicable to our implementation
* If applicable, whether the component can be removed, upgraded, recompiled, or otherwise remediated
* If immediate remediation isn’t feasible, the technical reason, any available mitigation, and the planned remediation approach

For example, my current understanding is that some of the zlib/log4j versions are associated with specific MATLAB runtimes, while the program containing the vulnerable Go stdlib can be removed.

The goal isn’t necessarily to immediately replace every older dependency. We need to be able to determine which vulnerability findings represent actual product/runtime exposure and document the technical disposition for each applicable critical finding.

Longer term, I’d like to make this a standard process so that when Security sends Engineering a new critical vulnerability finding, the required technical assessment is clear and we don’t have to rediscover the process for each CVE.







FossID vulnerability notification
             │
             ▼
1. SECURITY TRIAGE
             │
             ├── What CVE?
             ├── What severity/CVSS?
             ├── What product/SBOM?
             ├── What component?
             └── What version?
             │
             ▼
2. VALIDATE VULNERABILITY INFORMATION
             │
             ├── FossID
             ├── NVD / authoritative source
             ├── Affected version range
             └── Does detected version fall in affected range?
             │
             ▼
3. ENGINEERING APPLICABILITY ASSESSMENT
             │
             ├── Where is the component?
             ├── What application owns it?
             ├── Is it shipped/deployed?
             ├── Is it required at runtime?
             └── Is the vulnerability actually applicable?
             │
             ▼
4. DETERMINE DISPOSITION
             │
       ┌─────┼────────────┬───────────────┐
       ▼     ▼            ▼               ▼
   AFFECTED  NOT       NOT RUNTIME     AFFECTED BUT
             AFFECTED   / NOT SHIPPED   CANNOT FIX NOW
       │       │            │               │
       ▼       ▼            ▼               ▼
 Remediate   Document     Remove or       Justification
             evidence     exclude         + mitigation
                                         + remediation plan
             │
             ▼
5. DOCUMENT INVESTIGATION RESULT
             │
             ├── CVE
             ├── Component/version
             ├── Product
             ├── Engineering assessment
             ├── Applicability
             ├── Evidence
             ├── Disposition
             └── Remediation/justification
             │
             ▼
6. SECURITY / PSIRT REVIEW
             │
             ▼
7. CLOSE OR TRACK REMEDIATION






* Send Baba-san the workflow/ownership clarification message.
* Send Kurt/TMEA Engineering the technical assessment request.
* Wait for Baba-san to confirm who owns completion of the Vulnerability Investigation Results workbook.
* Wait for Engineering to provide component → path → application/runtime → deployed/runtime status → vulnerability applicability → remediation/justification information.
* Do not spend additional time investigating the temporary CVE/NVD discrepancy unless it occurs again or Baba-san specifically requests additional investigation.
* When Engineering responds, organize their findings by component and version rather than treating “Log4j,” “zlib,” or “Go” as one finding.
* Determine which detected versions are actually shipped/deployed and runtime-relevant.
* Determine which vulnerable versions require remediation versus documented justification.
* Use Engineering’s response to complete or contribute to the appropriate Vulnerability Investigation Results record once Baba-san confirms ownership.
* Review one completed Investigation Results workbook with Baba-san/Japan if possible to establish exactly what evidence they expect.
* Document the agreed Security → Engineering → PSIRT workflow after responsibilities are confirmed.
* Turn the agreed process into a short operating procedure/RACI so future vulnerability notifications follow the same workflow automatically.

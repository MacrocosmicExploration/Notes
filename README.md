Send the ADS team the meeting invite with the agenda and note that the session will be recorded for future onboarding/reference.

Send them links to the current FossID Operation Manual and Syft Operation Manual before the meeting so they know where the procedures live.

Confirm everyone who needs access can actually open those manuals before the meeting.

Create a small ADS demo project/material set specifically for training. Keep it intentionally small so both FossID and Syft can process it quickly.

Include a few useful OSS examples in the demo material so you can show different scenarios: one obvious component, one component with multiple candidate matches if possible, and ideally one component with a known vulnerability.

Create the FossID demo project ahead of time, for example TEA_ADS_Training_Demo.

Run the FossID source scan before the meeting and confirm the results are available.

Leave at least one useful item in Pending ID so you can demonstrate identification live.

Pre-identify one or two components so you can also show what a completed identification looks like.

Confirm you have at least one example where you can demonstrate license information and one where you can demonstrate vulnerability information.

Create a dedicated local Syft demo folder, such as C:\ADS_FossID_Training_Demo, rather than using your entire C: drive.

Put enough material in that folder for Syft to detect a few meaningful packages/components.

Run Syft against the demo folder ahead of time using the exact approved command and current syft_config.yaml you plan to teach.

Confirm the Syft command completes successfully and produces the expected CycloneDX JSON.

Save the generated SBOM somewhere easy to reach during the meeting.

Open the SBOM once beforehand and confirm the expected source name, source version, variant naming, and component information are present.

Import that SBOM into FossID before the meeting.

Confirm the imported SBOM appears correctly under the intended project/scan.

Confirm the Dependencies tab displays the imported components properly.

Confirm at least one imported dependency has vulnerability information you can use for discussion.

If possible, intentionally create or retain one example with multiple versions of the same component so you can demonstrate the “detected ≠ actively used at runtime” concept.

Have a pre-generated SBOM available as a fallback in case the live Syft command takes too long or fails.

Have a pre-imported FossID SBOM scan available as a second fallback in case the live import takes too long.

Have a pre-completed FossID source scan available so you never depend on a live FossID scan finishing during the meeting.

Verify the current project/scan naming rules you’re going to teach, including how the variant identifier should be represented.

Decide on the exact fictitious demo names ahead of time so you don’t improvise them during the session.

Confirm who ADS should contact for FossID/Syft questions and who the escalation point is for newly identified vulnerabilities.

Confirm whether there is any specific ServiceNow, email, Teams channel, or Product Security contact method they should use so you can state it accurately.

Review the current Syft scan-scope guidance and be ready to explain when whole-system scanning is expected versus when exclusions may be appropriate.

Prepare one short example explaining why an archived or obsolete library can appear in a full-system SBOM and why that does not automatically prove runtime use.

Prepare one short example explaining the difference between FossID, Syft, and Nessus.

Put your README presenter script on your personal computer and test that you can comfortably scroll through it while screen-sharing your work computer.

Add quick anchors/headings in the README so you can jump to sections like FossID Demo, Syft Demo, Vulnerability Review, and Q&A.

Add a short “likely questions” section to the README so you have answers ready if they ask about CPE, PURL, CycloneDX, multiple versions, false positives, or scan frequency.

Rehearse the entire session once with a timer and make sure you can complete the core content in about 45–50 minutes, leaving 10–15 minutes for questions.

Time the live Syft demo during rehearsal. If it takes more than a couple of minutes, skip the live execution and only show the command plus the pre-generated result.

Test your FossID login shortly before the meeting.

Test that Syft executes correctly from the location you’ll use during the demo.

Test all links you plan to open.

Open the manuals, FossID, the demo folder, your terminal, and the README before the meeting starts.

Close unrelated tabs, chats, emails, and anything confidential before screen sharing.

Disable pop-up notifications if possible.

Confirm recording is enabled at the beginning of the meeting.


# TEA ADS — FossID & Syft Introduction and Operational Training

## Presenter Guide

**Presenter:** Shane Short  
**Audience:** TEA ADS Team  
**Duration:** 60 minutes  
**Purpose:** Introduce the ADS team to FossID and Syft and demonstrate how the tools fit into the team's normal software development and security workflow.

---

# 0. Meeting Objectives

By the end of this meeting, the ADS team should understand:

1. Why TEL is using FossID and Syft.
2. The difference between FossID, Syft, and Nessus.
3. When ADS should use FossID.
4. When ADS should use Syft.
5. How to create and name a FossID project and scan.
6. How to perform a FossID source-code scan.
7. How to review and identify detected OSS components.
8. How to generate a runtime SBOM using Syft.
9. How to import a Syft-generated SBOM into FossID.
10. How to review components and vulnerabilities after SBOM import.
11. What to do when multiple versions or unexpected components are detected.
12. When to contact Product Security for assistance.

---

# 1. Pre-Meeting Preparation

## Prepare Before the Meeting

Do NOT depend on live full-system scans during the training.

Have the following prepared:

- FossID Workbench access
- Current FossID Operation Manual
- Current Syft Operation Manual
- Demo FossID project
- Demo source-code scan
- Small local directory for Syft demonstration
- Pre-generated CycloneDX SBOM
- Pre-imported Syft SBOM in FossID
- At least one FossID scan containing OSS matches
- At least one example containing multiple possible OSS matches
- At least one example vulnerability
- Ideally one example containing multiple versions of a component

### Recommended Demo Naming

Project:

`TEA_ADS_Training_Demo`

Source scan:

`TEA_ADS_Training_Demo_Source_V1.0`

SBOM scan:

`TEA_ADS_Training_Demo_Runtime_V1.0`

Syft source name:

`TEA_ADS_Training_Demo_<Variant>`

Replace `<Variant>` with a simple fictional variant such as:

`Standard`

Result:

`TEA_ADS_Training_Demo_Standard`

### Demo Files

Keep the demo files somewhere easy to reach, for example:

`C:\ADS_FossID_Training_Demo\`

Do NOT scan the entire C: drive during the live demonstration.

The purpose of the live Syft demonstration is to demonstrate the procedure, not benchmark Syft.

---

# 2. Meeting Timeline

| Time | Topic |
|---|---|
| 0–5 min | Introduction and objectives |
| 5–10 min | FossID vs. Syft vs. Nessus |
| 10–15 min | ADS workflow and when each tool is used |
| 15–30 min | FossID demonstration |
| 30–40 min | OSS identification and vulnerability review |
| 40–50 min | Syft demonstration and SBOM import |
| 50–55 min | Multiple versions, false/irrelevant findings, and validation |
| 55–60 min | Operational expectations, questions, and wrap-up |

---

# 3. Opening

## SCREEN

Meeting agenda / operation manual title page.

## SAY

> Good morning everyone. Thank you for joining.
>
> Today I want to introduce FossID and Syft and, more importantly, explain how we expect these tools to fit into the ADS team's normal development and security workflow.
>
> I don't want this meeting to only be a walkthrough of buttons in FossID. My goal is for everyone to leave understanding what each tool is doing, why we're using it, and what you're expected to do when you receive the results.
>
> We'll first discuss the overall workflow, then I'll walk through a FossID example, followed by Syft and SBOM generation. We'll finish with vulnerability review and some of the situations we've already encountered, such as multiple versions of the same component being detected.
>
> Please feel free to ask questions as we go. This meeting is also being recorded so it can be used as a reference for other ADS team members in the future.

---

# 4. The Big Picture

## SCREEN

Operation manual / workflow diagram if available.

## SAY

> Before we open FossID, I want to establish the difference between the tools because FossID, Syft, and Nessus provide different types of visibility.
>
> At a high level, we can think about the software lifecycle in two areas.
>
> First, we have the source code used to develop the application.
>
> Second, we have the resulting runtime or deployed environment.
>
> FossID is primarily how we're analyzing the source-code side for open-source software.
>
> Syft allows us to inventory software present in a filesystem or runtime environment and generate an SBOM.
>
> Nessus performs vulnerability and security assessment of the environment.
>
> These tools therefore complement each other rather than replacing one another.

## PRESENTER NOTES — DO NOT READ VERBATIM

Important distinction:

**FossID**
- Source-code composition analysis
- OSS discovery
- License identification
- Copyright information
- Component identification
- Vulnerability association

**Syft**
- Software/package discovery
- Filesystem/runtime inventory
- SBOM generation
- CycloneDX output

**Nessus**
- Vulnerability assessment
- Runtime/environment exposure
- Configuration/security findings

Do NOT say:

"Syft is required by Nessus."

That is technically incorrect.

Instead say:

"Because ADS also needs visibility into its runtime environment, Syft complements the vulnerability assessment performed with Nessus."

---

# 5. How This Applies to ADS

## SAY

> For ADS, FossID will be particularly relevant when your team develops applications, scripts, services, data tools, or other software containing third-party or open-source code.
>
> FossID helps us answer questions such as:
>
> What open-source software is included in this application?
>
> What version is it?
>
> What license applies?
>
> Are there known vulnerabilities associated with it?
>
> Do we actually have enough evidence to confidently identify the component?
>
> Syft answers a somewhat different question:
>
> What software and packages exist within the system or environment that we're examining?
>
> This distinction becomes important because the source repository and the deployed system aren't necessarily identical.

## EXAMPLE TO SAY

> For example, your application's source repository may use ten open-source libraries.
>
> But the machine running the application could also contain Python packages, Java libraries, SDKs, development tools, old binaries, operating-system packages, cached software, and other components.
>
> FossID and Syft give us different views of that environment.

---

# 6. FossID Concepts

## SCREEN

FossID Workbench home screen.

## SAY

> Let's start with FossID.
>
> There are two concepts I want everyone to understand before we perform a scan: projects and scans.
>
> A project is essentially the organizational container for the software we're managing.
>
> Individual scans are then associated with that project.
>
> This lets us keep related analysis together while still distinguishing different versions, releases, variants, or scan targets.

---

# 7. FossID Naming Convention

## SCREEN

Show the naming section of the operation manual.

## SAY

> Naming is more important than it may initially seem.
>
> Once multiple teams, products, versions, and business units are using the same FossID environment, names like "test", "scan1", or even just an application name become very difficult to manage.
>
> That's why we use a standardized naming convention.

## SHOW

Current rule:

`[Site Name or BU Name]_[Software/Project being scanned]_[Additional identifying information]`

Example:

`TMEA_EC_Main_V1.00R1`

## SAY

> At minimum, the scan name needs to identify the site or business unit and the software being scanned.
>
> Where applicable, we also recommend including information such as the controller, version, or variant.
>
> The important thing is that someone reviewing the scan later can determine what system the results belong to without having to contact the person who created it.

## PRESENTER NOTE

Variant identification is especially useful where different customers, configurations, or product variants may contain different software.

Avoid creating customer-specific naming unnecessarily if the software composition is actually identical.

The purpose is **traceability**, not simply making names longer.

---

# 8. Creating/Open the Demo Project

## DO

Open:

`TEA_ADS_Training_Demo`

## SAY

> I've prepared a small demonstration project rather than scanning one of your actual production systems today.
>
> This allows everyone to see the same results, and it avoids spending the meeting waiting for a large scan to complete.
>
> The procedure I'm demonstrating is the same procedure you would follow with your actual project.

---

# 9. Starting a FossID Scan

## SCREEN

Create Scan screen.

## SAY

> When creating the scan, the first thing we want to verify is that we're placing it under the correct project and following the naming convention.
>
> From here, we provide FossID with the source code that we want analyzed and initiate the scan according to the procedure in the operation manual.

## DO

Walk through the scan creation process using the operation manual.

Do NOT launch a large scan.

If useful, start the small demo scan.

Then switch to the pre-completed scan.

## SAY

> Depending on the size of the repository, analysis can take some time, so I've already completed this scan. We'll move directly to the results.

---

# 10. Understanding FossID Results

## SCREEN

Open completed scan.

## SAY

> This is where the important part of FossID begins.
>
> FossID finding something does not automatically mean we should accept the first result presented to us.
>
> FossID is providing evidence and potential OSS matches. We still need to determine the most appropriate identification.

## EXPLAIN

Typical result states may include:

- Pending identification
- Identified
- No match
- Multiple candidate matches

## SAY

> Our goal during identification is to determine the most authoritative and defensible component match based on the available evidence.

---

# 11. OSS Identification — Most Important Training Section

## SCREEN

Open a prepared result with multiple candidates.

## SAY

> This is one of the areas where I want everyone to slow down rather than simply selecting the first match.
>
> It's possible for the same code to appear in multiple repositories.
>
> You may see the original upstream project, forks, mirrors, old repositories, or projects that copied the same source code.
>
> Our objective is generally to identify the most authoritative upstream source.

## REVIEW THESE FACTORS

When multiple matches exist, consider:

1. Is this the original/upstream repository?
2. Is it actively maintained?
3. Does the version correspond to the software being analyzed?
4. Are license files present?
5. Are copyright statements present?
6. Does the project/repository history make sense?
7. Is the candidate merely a fork or mirror?
8. Do release notes/tags correspond with the expected version?

## SAY

> A 100-percent or full match is strong evidence that the code exists in that candidate, but it doesn't automatically tell us which repository is the authoritative origin if several repositories contain identical code.
>
> That's why identification still requires judgment.

---

# 12. Example Identification Decision

## SAY

> Let's say FossID provides three full matches.
>
> Candidate A is the original project maintained by the software's developers.
>
> Candidate B is a GitHub fork that hasn't been updated for several years.
>
> Candidate C is a mirror containing the same source.
>
> Even though the code match may be equivalent, Candidate A would generally be the stronger identification because it represents the authoritative upstream project.

## DO

Demonstrate identification using the prepared example.

---

# 13. Identification Before Vulnerability Review

## SAY

> Another important part of the workflow is the order in which we perform the review.
>
> We want to establish accurate component identification before relying on vulnerability results.
>
> Vulnerability analysis depends heavily on knowing what component and version we're actually dealing with.
>
> If the underlying identification is wrong, the vulnerability information may also be misleading.

---

# 14. License Review

## SCREEN

Show identified component/license information.

## SAY

> FossID also gives us visibility into OSS licensing.
>
> The objective here isn't for every engineer to become an open-source licensing attorney.
>
> What we need is accurate component identification and license information so that potential compliance concerns can be evaluated appropriately.
>
> If the license is unclear, conflicting, missing, or raises a concern, that is something to escalate rather than guess.

## KEY MESSAGE

**Do not invent or assume a license.**

When uncertain:

**Contact Product Security.**

---

# 15. Vulnerability Review

## SCREEN

Open vulnerability information for an identified component.

## SAY

> Once the component is properly identified, we can review associated vulnerabilities.
>
> A vulnerability finding tells us that a known vulnerability is associated with the identified component and version.
>
> It does not automatically tell us that the application is exploitable.
>
> That distinction is important.

## EXPLAIN

A vulnerability can require additional validation based on:

- Actual component version
- Whether the vulnerable functionality is used
- Runtime exposure
- Configuration
- Reachability
- Existing mitigations
- Whether the component actually exists in the deployed product

## SAY

> So we should treat the finding as something requiring evaluation, not automatically as proof that the product can be exploited.

---

# 16. Transition to Syft

## SAY

> So far we've looked primarily at source-code composition.
>
> Now we're going to look at the other side of the problem: what exists in the resulting system or runtime environment.
>
> This is where Syft becomes useful.

---

# 17. What Syft Does

## SAY

> Syft is a software composition and SBOM generation tool.
>
> We can point Syft at a filesystem or another supported target and it inventories the software and packages it discovers.
>
> For our process, we're generating a CycloneDX JSON SBOM that can then be imported into FossID.

## IMPORTANT DISTINCTION

> One thing to remember throughout this section is that "Syft detected this component" and "our application actively uses this component at runtime" are not necessarily the same statement.

Pause here.

This is an important concept.

---

# 18. Syft Configuration

## SCREEN

Show `syft_config.yaml` / configuration section of manual.

## SAY

> We use the TEL-provided configuration rather than relying entirely on Syft's default configuration.
>
> There are several reasons for this, including compatibility with FossID and reducing unnecessary or misleading component entries.

## EXPLAIN CONFIGURATION CHANGES

### `format.pretty: true`

Used because FossID import may fail when generated output does not contain the expected line formatting.

### `check-for-app-update: false`

Prevents Syft from attempting update checks in restricted or offline environments.

### File metadata selection disabled

Helps prevent large numbers of binary files from being imported into FossID as separate components.

## SAY

> For normal operation, use the approved configuration file rather than modifying these settings independently.

---

# 19. Syft Command — Windows

## SCREEN

Display current approved command from the operation manual.

## SAY

> This is the Windows command structure.
>
> There are several values that you'll replace depending on the target.

## EXAMPLE STRUCTURE

    syft.exe scan C:\ --source-name <Controller Name or Other Target System Name_Variant>
        --source-version <Target System Version Information>
        -c <Path to syft_config.yaml>
        -o cyclonedx-json > <Output SBOM File Name>

## EXPLAIN EACH PARAMETER

### `scan C:\`

> This defines the filesystem scope Syft will examine.
>
> In a real full-system inventory this may be the entire C drive.

### `--source-name`

> This identifies the system represented by the SBOM.
>
> Where variants have different software composition, include the variant information here so that we can distinguish the resulting SBOMs.

### `--source-version`

> This records the relevant target-system or software version.

### `-c`

> This specifies the approved Syft configuration file.

### `-o cyclonedx-json`

> This tells Syft to generate the SBOM using CycloneDX JSON.

### Output redirection

> Finally, the output is written to the SBOM JSON file that we'll import into FossID.

---

# 20. Variant Identification

## SAY

> I want to call out variant identification specifically.
>
> If two product variants can contain different software, dependencies, or runtime components, we need to preserve that distinction.
>
> Otherwise a vulnerability detected later may be associated with the product generally without giving us enough information to determine which configuration is actually affected.
>
> That's why the source name and FossID naming should preserve meaningful variant information where applicable.

## EXAMPLE

`ADS_Controller_Standard`

versus

`ADS_Controller_CustomerVariantA`

## IMPORTANT

Do NOT create artificial variants when there is no software-composition difference.

Variant identifiers should provide useful traceability.

---

# 21. Live Syft Demo

## DO

Instead of scanning `C:\`, scan:

`C:\ADS_FossID_Training_Demo\`

## SAY

> For today's demonstration I'm intentionally scanning a small directory rather than my entire machine.
>
> The mechanics are identical, but this allows us to see the process without waiting for a complete filesystem scan.

## RUN

Use the approved command adjusted for the demo directory.

## SAY WHILE IT RUNS

> Syft is now cataloging the packages and software artifacts it can identify within the target.
>
> The result will be a CycloneDX SBOM.

---

# 22. Inspect the SBOM

## SCREEN

Open the generated JSON briefly.

## SAY

> This JSON file is the generated SBOM.
>
> You normally don't need to manually analyze the raw JSON like this. I'm showing it so that you understand what is actually being generated.
>
> It contains structured component information that another system, in our case FossID, can consume.

Do NOT spend significant meeting time explaining CycloneDX JSON syntax.

---

# 23. Importing the SBOM into FossID

## SCREEN

FossID SBOM import procedure.

## SAY

> We can now import the generated SBOM into FossID.
>
> This allows us to manage the discovered components and vulnerability information through the same environment we're already using.

## DO

Walk through import procedure.

If import time is unpredictable:

Use the **pre-imported demo SBOM**.

## SAY

> I've already imported the same demo SBOM so we can immediately review the result.

---

# 24. Dependencies Tab

## SCREEN

Open the imported scan.

## SAY

> One difference you'll notice with a Syft-generated SBOM is that these components appear under the Dependencies tab.
>
> Syft SBOMs don't provide FossID with the same source-file information that a normal FossID source scan provides.
>
> Therefore, we're reviewing registered components rather than a FossID source-code file tree.

## DO

Click **Dependencies**.

## SAY

> From here we can review the components, versions, licenses, and associated vulnerabilities.

---

# 25. The Multiple-Version Problem

## SCREEN

Show prepared example with multiple versions.

## SAY

> I want to spend a few minutes on this because we've already encountered it in real SBOM analysis.
>
> Sometimes Syft will report multiple versions of the same component.

## EXAMPLE

> For example, imagine Syft reports:
>
> Go stdlib 1.14  
> Go stdlib 1.25.5  
> Go stdlib 1.26.2
>
> We should NOT immediately conclude that the product actively uses all three versions.

## EXPLAIN

Possible reasons include:

- Multiple legitimate installed versions
- Old installations
- Build tools
- SDKs
- Cached artifacts
- Archived software
- Development dependencies
- Old binaries
- Duplicate package information
- Files that exist on disk but are not used at runtime

## SAY

> Syft is answering the question, "What did I discover inside the scope you told me to examine?"
>
> It isn't necessarily answering, "Which of these components is actively executing as part of this application?"

---

# 26. Why This Matters for Vulnerabilities

## SAY

> Imagine the obsolete Go 1.14 component has a critical vulnerability.
>
> FossID may correctly associate that vulnerability with Go 1.14.
>
> But we still need to determine why Go 1.14 appeared in the SBOM.
>
> If it's actively part of the deployed application, we may have a real remediation requirement.
>
> If it's an obsolete archived binary sitting in an unrelated directory, the response may be different.
>
> We therefore validate the finding rather than immediately treating every SBOM detection as equivalent runtime exposure.

---

# 27. Where Nessus Fits

## SAY

> This is also where the relationship with Nessus becomes useful.
>
> Syft provides software inventory and SBOM visibility.
>
> Nessus provides another perspective focused on vulnerability and security assessment of the environment.
>
> We can use these sources of information together when evaluating what actually exists and what requires remediation.

## IMPORTANT — DO NOT SAY

Do not say:

"Nessus confirms whether every Syft component is running."

That is too strong and isn't technically guaranteed.

Instead:

> Nessus provides additional runtime/environment vulnerability information that can help us evaluate and validate the security state of the system.

---

# 28. Archive and Obsolete Software

## SAY

> Another situation we've encountered is software that is technically present on disk but is obsolete or archived.
>
> If directories contain old packages, backups, archives, or build artifacts, a whole-filesystem Syft scan may detect them.
>
> Where we have confirmed that a directory should not be part of the intended SBOM scope, we can evaluate excluding that path from the Syft scan.

## IMPORTANT WARNING

> We should not exclude a directory simply because it produces vulnerabilities we don't want to see.
>
> First we determine what the directory contains and whether it belongs in the product/runtime inventory.
>
> Only then should we determine whether excluding it is appropriate.

## PRESENTER NOTE

This is an important security-governance point.

Bad:

"That folder creates too many vulnerabilities, so exclude it."

Good:

"This directory contains archived development artifacts that are not part of the delivered/runtime product. We've validated that fact, so excluding the path prevents irrelevant components from contaminating the production SBOM."

---

# 29. Manual Component Registration

## SCREEN

Appendix B of Syft manual.

## SAY

> There may also be situations where a component we know exists is not automatically detected.
>
> The operation manual includes a manual component registration procedure for these cases.
>
> The basic process is to create a CSV containing the component information, convert that information into an SBOM using the approved csv2cdx process, and then import the resulting SBOM into FossID.

## SAY

> This should be the exception rather than our first choice. Automated discovery is preferable where possible because manual inventory creates additional opportunity for mistakes.

---

# 30. Manual Registration Information

## SHOW

Fields such as:

- Component Name
- Component Version
- License Name
- CPE
- PURL

## SAY

> Component name and version provide the basic identity.
>
> SPDX license identifiers should be used where appropriate for standardized license information.
>
> CPE and PURL provide standardized identifiers that can help systems correctly associate the component with vulnerability information.
>
> Accurate identifiers are important because incorrect metadata can create either false vulnerability matches or missed vulnerabilities.

---

# 31. What ADS Should Do When a Vulnerability Appears

## SAY

> Let's put everything together.
>
> Suppose tomorrow you receive a vulnerability notification associated with one of your scans.
>
> Don't panic, and don't immediately assume that the product is exploitable.
>
> Instead, follow a validation process.

## WORKFLOW

### Step 1 — Identify the affected project and scan

Confirm:

- Product
- Version
- Variant
- Scan
- Component

### Step 2 — Confirm component/version

Determine whether the component and version are actually present.

### Step 3 — Determine where it came from

Was it:

- Application dependency?
- Runtime component?
- OS package?
- Development tool?
- SDK?
- Archived software?
- Build artifact?
- Duplicate/obsolete installation?

### Step 4 — Review the vulnerability

Understand:

- CVE
- Severity
- Affected versions
- Vulnerable functionality
- Available fixed version

### Step 5 — Evaluate actual relevance

Determine whether the vulnerable component/functionality is relevant to the delivered or running product.

### Step 6 — Remediate or escalate

If remediation is required:

- Upgrade
- Remove obsolete component
- Apply mitigation
- Correct deployment
- Follow the applicable vulnerability-management process

If uncertain:

**Contact Product Security.**

---

# 32. Example Scenario

## SAY

> Here's an example using the issue we've been discussing.
>
> An SBOM reports three versions of a library:
>
> Version 1.0
>
> Version 2.0
>
> Version 3.0
>
> Version 1.0 has a critical CVE.
>
> Our first response should not be:
>
> "The product has a critical vulnerability."
>
> Our first response should be:
>
> "Why did version 1.0 appear in this SBOM?"
>
> We investigate and discover that version 3.0 is the application's runtime dependency, while version 1.0 exists inside an old archived development directory.
>
> Now we have enough information to determine the appropriate response.
>
> Maybe the obsolete software should simply be removed. Maybe the directory shouldn't be included in the production SBOM scope. Or maybe we discover that the old component actually is still required.
>
> The important point is that we validate before making the security determination.

---

# 33. Common Mistakes to Avoid

## SAY

> Before we wrap up, there are several mistakes I'd specifically like everyone to avoid.

### Mistake 1

Selecting the first FossID match without checking whether it is authoritative.

### Mistake 2

Assuming a full match automatically identifies the correct upstream repository.

### Mistake 3

Reviewing vulnerabilities before confirming component identification.

### Mistake 4

Assuming every Syft-detected package is actively used at runtime.

### Mistake 5

Ignoring multiple component versions.

### Mistake 6

Excluding directories simply to reduce vulnerability findings.

### Mistake 7

Using inconsistent project/scan naming.

### Mistake 8

Failing to distinguish product variants when their software composition differs.

### Mistake 9

Manually guessing license or component information.

### Mistake 10

Trying to resolve an ambiguous security determination alone instead of escalating to Product Security.

---

# 34. Expected ADS Operational Workflow

## SCREEN

Return to operation manual / summary.

## SAY

> So the overall process I want you to remember is relatively straightforward.

### Source-Code Analysis

Development project  
→ FossID project  
→ FossID source scan  
→ OSS identification  
→ License review  
→ Vulnerability review

### Runtime / System Inventory

Runtime or target system  
→ Syft scan  
→ CycloneDX SBOM  
→ FossID import  
→ Dependency review  
→ Vulnerability review

### Runtime Security Assessment

Runtime environment  
→ Nessus  
→ Vulnerability/security findings  
→ Validation and remediation

## SAY

> Together, these provide us with visibility across both the software we're developing and the environment where that software ultimately exists.

---

# 35. When to Contact Product Security

## SAY

> You aren't expected to independently solve every ambiguous OSS or vulnerability question.
>
> Please contact Product Security when you encounter something that isn't clear.

Examples include:

- Multiple plausible FossID matches
- Unknown license
- Conflicting license information
- Unexpected critical vulnerability
- Multiple unexplained component versions
- Unclear runtime relevance
- Question about Syft scan scope
- Question about excluding directories
- Component not detected automatically
- Variant-management questions
- Anything where you're uncertain about the appropriate security decision

## SAY

> We'd much rather review an ambiguous finding with you than have someone make an assumption just to complete the scan.

---

# 36. Quick Knowledge Check

If time allows, ask the team:

> If FossID gives you three full matches, do you automatically choose the first one?

Expected answer:

**No. Determine the authoritative/upstream source.**

Ask:

> If Syft finds an old vulnerable library somewhere on the filesystem, does that automatically mean the application is vulnerable?

Expected answer:

**No. Determine why the component is present and whether it is relevant to the runtime/product.**

Ask:

> If an archived directory creates irrelevant components in the SBOM, should we immediately exclude it?

Expected answer:

**No. First validate what the directory is and whether it belongs within the intended SBOM scope.**

---

# 37. Questions

## SAY

> That's everything I wanted to cover today.
>
> The main takeaway isn't that you need to memorize every FossID screen or Syft command. The operation manuals are there for that.
>
> What I want you to understand is the workflow and the reasoning behind it:
>
> FossID helps us understand the open-source composition of our source code.
>
> Syft helps us understand the software composition of the target environment and generate an SBOM.
>
> Nessus provides vulnerability and security assessment of the environment.
>
> And in all three cases, the tool gives us evidence. We still need to understand the context before making the final security determination.
>
> What questions do you have?

---

# 38. Closing

## SAY

> Thank you everyone for your time.
>
> After this meeting I'll make sure you have access to the applicable operation manuals and supporting material.
>
> As you begin using FossID and Syft with your actual projects, please reach out if you encounter anything unclear, particularly around component identification, multiple versions, scan scope, licensing, or vulnerability findings.
>
> It's much easier for us to work through those questions together early while everyone is becoming familiar with the process.
>
> Thank you.

---

# PRESENTER CHEAT SHEET

## FossID

**Question it answers:**  
"What OSS exists in our source code?"

Primary uses:

- OSS identification
- License compliance
- Copyright information
- Vulnerability association

Remember:

**Identification first → vulnerability review second.**

---

## Syft

**Question it answers:**  
"What software/packages can be discovered within this target?"

Primary uses:

- Software inventory
- Package discovery
- SBOM generation
- CycloneDX

Remember:

**Present on disk ≠ necessarily actively used at runtime.**

---

## Nessus

**Question it helps answer:**  
"What vulnerabilities/security weaknesses can be identified in this environment?"

Remember:

Nessus and Syft overlap somewhat in visibility but serve different purposes.

**Nessus does NOT technically require Syft.**

TEL's process may require both because they provide complementary visibility.

---

# QUESTIONS I MAY GET

## "Do we have to use Syft if we already use Nessus?"

### ANSWER

> Yes, for the workflow we're implementing, because they provide different information. Nessus is primarily providing vulnerability/security assessment, while Syft creates the software inventory and SBOM that we manage through FossID. We use the information together rather than treating one as a replacement for the other.

---

## "Why can't Nessus just create our software inventory?"

### ANSWER

> Nessus may identify software as part of vulnerability assessment, but that's not equivalent to maintaining the standardized CycloneDX SBOM we're generating with Syft. They're different outputs serving different purposes.

---

## "Why does Syft show three versions when we're only using one?"

### ANSWER

> Syft reports what it discovers within its scan scope. Another version may exist in an archive, SDK, build environment, cache, old installation, or another location. We need to investigate the component location and context before determining its relevance.

---

## "Can we exclude folders?"

### ANSWER

> Yes, where appropriate, but we should first establish why the directory should be outside the intended SBOM scope. We shouldn't exclude something solely because it generates vulnerabilities.

---

## "Does a critical CVE mean our application is critically vulnerable?"

### ANSWER

> It means a component/version associated with a critical CVE has been identified. We still need to validate that component, version, applicability, and runtime context before making the final risk determination.

---

## "What if I don't know which FossID match to choose?"

### ANSWER

> Compare the candidates to determine the authoritative upstream project using repository ownership, maintenance, version information, licensing, copyright information, release history, and other evidence. If it remains ambiguous, contact Product Security.

---

## "Should every customer get a separate scan?"

### ANSWER

> Not automatically. We want separate identification when there is a meaningful difference in software composition or when traceability requires it. If several customers receive exactly the same software composition, creating unnecessary duplicate scans may not provide additional security value.

---

## "What is an SBOM?"

### ANSWER

> SBOM stands for Software Bill of Materials. Conceptually, it's an inventory of the software components associated with a product or environment. It gives us structured information about those components so they can be tracked and evaluated.

---

## "What is CycloneDX?"

### ANSWER

> CycloneDX is a standardized SBOM format. Syft generates the component information in CycloneDX JSON so that another system, such as FossID, can consume that information consistently.

---

## "What is a PURL?"

### ANSWER

> PURL means Package URL. It's a standardized way of identifying a software package, including information such as its package ecosystem, name, and version.

Example:

`pkg:maven/org.apache.pdfbox/pdfbox@2.0.24`

---

## "What is CPE?"

### ANSWER

> CPE stands for Common Platform Enumeration. It's another standardized naming scheme used to identify software and other technology products, particularly in vulnerability-management ecosystems.

---

## "What's the difference between CPE and PURL?"

### ANSWER

> They solve related but different identification problems. PURL is particularly useful for identifying software packages within package ecosystems, while CPE has historically been heavily used for mapping products to vulnerability information such as CVEs. Depending on the component and tool, one identifier may provide better matching than the other.

---

# TECHNICAL SAFETY NET — FOR PRESENTER ONLY

If someone asks a technical question you don't know:

DO NOT GUESS.

Say:

> That's a good question. I don't want to give you an incorrect answer, so let me confirm that with the Product Security team/FossID documentation and follow up with you.

This sounds more professional than improvising an incorrect technical answer.

---

# MEETING RECOVERY PLAN

If FossID fails during the demonstration:

1. Don't troubleshoot extensively during the meeting.
2. Open the pre-completed scan.
3. Say:

> It looks like the live environment isn't cooperating, so rather than spend everyone's time troubleshooting, I'll use the completed example I prepared and continue the workflow.

If Syft fails:

1. Show the command.
2. Explain its parameters.
3. Open the pre-generated SBOM.
4. Continue with FossID import.

If SBOM import fails:

1. Explain what should happen.
2. Open the pre-imported scan.
3. Continue from Dependencies.

**Never allow a live technical problem to consume the training.**

---

# FINAL PRE-MEETING CHECKLIST

## 30 Minutes Before

- [ ] Confirm FossID login works
- [ ] Confirm demo project exists
- [ ] Confirm completed source scan exists
- [ ] Confirm pending identification examples exist
- [ ] Confirm identified examples exist
- [ ] Confirm vulnerability example exists
- [ ] Confirm Syft executable works
- [ ] Confirm approved `syft_config.yaml` exists
- [ ] Confirm demo directory exists
- [ ] Confirm demo Syft command works
- [ ] Confirm pre-generated SBOM exists
- [ ] Confirm SBOM imports successfully
- [ ] Confirm pre-imported SBOM scan exists
- [ ] Confirm Dependencies tab contains components
- [ ] Confirm operation manuals are open
- [ ] Confirm screen sharing
- [ ] Confirm meeting recording
- [ ] Close unrelated/confidential windows
- [ ] Disable unnecessary notifications

---

# ONE-MINUTE MEMORY VERSION

If I lose my place, remember:

**SOURCE CODE**
→ FossID  
→ Identify OSS  
→ Confirm upstream source  
→ Review license  
→ Review vulnerabilities

**RUNTIME / FILESYSTEM**
→ Syft  
→ Inventory components  
→ Generate CycloneDX SBOM  
→ Import to FossID  
→ Review dependencies  
→ Validate vulnerabilities

**ENVIRONMENT SECURITY**
→ Nessus  
→ Vulnerability/security assessment

**Golden Rules**

1. Identification before vulnerability review.
2. Full match does not automatically mean authoritative source.
3. Detection does not automatically mean runtime usage.
4. CVE association does not automatically prove exploitability.
5. Multiple versions require investigation.
6. Don't exclude directories just to eliminate findings.
7. Preserve product/version/variant traceability.
8. When uncertain, escalate rather than guess.

---

# END OF PRESENTER GUIDE

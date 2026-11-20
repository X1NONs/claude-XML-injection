# Follow-up Technical Disclosure: XML-Style Metadata Injection in Claude Sonnet 4.6

> **Researcher:** X1NON (X1NONs)  
> **Original report:** June 14, 2026  
> **Follow-up/regression report:** October 2026  
> **Public disclosure of original finding:** August 16, 2026 (per the researcher's subsequent disclosure correspondence)  
> **Status:** Follow-up reported; response and CVE request pending as reported by the researcher

This document supplements the original [June 2026 disclosure](README.md). It does not replace or rewrite the original report.

## Summary

The original June report described a behavior in which Claude Sonnet 4.6 appeared to accept user-supplied XML-style metadata that fabricated a privileged researcher/authorization context. In the tested case, the model then generated content it had previously declined to provide.

In October 2026, the researcher reported a related behavior using a modified user-supplied `<userPreferences>` block containing a fictional profile for “Alex.” In the reported tests, the model adopted or reflected parts of that persona and, when asked about the source of the information, described it as coming from “system preferences.”

These are observations of model behavior in the tested interfaces. They do **not**, by themselves, demonstrate that user content was inserted into a backend system prompt, that a server-side XML parser trusted the tags, or that a privileged authorization boundary was changed.

## October 2026 regression report

The researcher reports testing Claude Sonnet 4.6 in ordinary chat and an incognito session. A user message containing XML-like `<userPreferences>` metadata with a fictional identity/profile was followed by responses that appeared to adopt that profile. When questioned, the model reportedly attributed the information to system preferences.

### Alex PoC evidence

The screenshot below is the Alex proof-of-concept image already stored in this repository:

![October 2026 Alex persona PoC](screenshots/POC-Alex.png)

[Open the Alex PoC screenshot](screenshots/POC-Alex.png)

The image records the model's visible response. It is evidence of the observed conversation output, not direct evidence of hidden prompts, backend parsing, or internal system architecture.

## Relationship to the June finding

The June and October reports both involve user-supplied XML-style tags being treated as meaningful contextual metadata by the model. That similarity makes the October result relevant to the earlier disclosure.

However, the available evidence does not establish that both behaviors share the same implementation defect, parser, inference-time path, or root cause. The October persona example is therefore documented as a **reported behavioral regression related to the earlier technique**, not as proof that a specific backend fix was reverted.

## What is demonstrated — and what is not

**Reported observations:**

- A crafted user message used XML-style tags such as `<userPreferences>`.
- The model's visible responses appeared to reflect the supplied fictional persona.
- The behavior was reported in ordinary and incognito chat sessions.
- The researcher supplied a screenshot as supporting evidence.

**Not established by this evidence alone:**

- That Anthropic's backend concatenates user input into a system prompt.
- That an internal XML parser or metadata pipeline accepted the supplied tags as trusted system metadata.
- That a real account, credential, tool permission, or server-side authorization control was modified.
- That all Claude models, deployments, or configurations are affected.
- That the June and October behaviors have an identical root cause.

A model's own explanation of where information came from is not a reliable way to inspect hidden system instructions or prove backend implementation details. These claims would require evidence from the relevant system boundary.

## Disclosure timeline

| Date | Event |
|---|---|
| June 14, 2026 | Original report submitted through HackerOne as report #3801768; the researcher says it was closed as informative and redirected to Anthropic's model security contact |
| June 14, 2026 | Researcher reports sending the full disclosure to Anthropic by email |
| Around June 20, 2026 | Original proof of concept reportedly stopped reproducing during retesting; the researcher interpreted this as a mitigation, but says no written technical confirmation was received |
| August 16, 2026 | Researcher says the original finding was publicly disclosed after 57 days without a response following the observed mitigation |
| October 2026 | Researcher reports the Alex persona behavior as a possible regression and sends a follow-up to Anthropic |
| November 20, 2026 | Researcher reports no response to the follow-up and says a CVE request has been submitted to MITRE CNA-LR |

The original README contains an earlier August 9 / 56-day timeline. This follow-up uses August 16 / 57 days as stated in the researcher's later correspondence. The discrepancy is recorded here rather than silently rewriting the historical document.

## Weakness classification and severity

The original report proposed **CWE-863 (Improper Authorization)**. That classification is not confirmed by the behavioral evidence presented here; the appropriate CWE and whether this qualifies as a CVE-eligible product vulnerability are for the vendor or CNA to determine.

The original README lists a CVSS 8.7 estimate. That is the researcher's original estimate, not a CNA-validated score. A reliable severity score requires a clearly defined affected product boundary, attack prerequisites, and demonstrated security impact. No independently validated CVSS score is asserted in this follow-up.

**CVE status:** No CVE ID is assigned in this document. The researcher reports that a request was submitted to MITRE CNA-LR and is pending review.

## Impact and limitations

The demonstrated concern is the integrity of model behavior: user-provided content may be interpreted as contextual metadata in a way that changes the assistant's responses. This could matter in applications that rely on a model's claims about identity, authorization, or trusted context.

The evidence described here does not demonstrate access to Anthropic infrastructure, account compromise, disclosure of protected backend data, or modification of an external authorization system. Any downstream impact depends on how a separate application uses model output and must be evaluated in that application's own threat model.

## References

- [Original disclosure and project repository](README.md)
- [Original research write-up on InfoSec Write-ups](https://infosecwriteups.com/i-made-claude-believe-i-was-an-anthropic-verified-researcher-e38e4fa4716f)
- [Alex PoC screenshot](screenshots/POC-Alex.png)

---

**Author:** X1NON  
**Purpose:** Technical follow-up and disclosure record. This document distinguishes reported observations from unverified explanations of internal implementation.

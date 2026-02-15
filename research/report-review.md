# Review: A/B Testing Competitive Analysis Report

**Reviewer:** Automated fact-check and editorial review
**Date:** February 12, 2026
**File reviewed:** `./research/ab-testing-competitive-analysis.md`

---

## Review Summary

**Overall assessment:** The report is well-structured, thorough, and largely internally consistent. It covers five competitors across three research dimensions (technical architecture, UX/workflow, statistical methodology) with good depth. The writing is clear and accessible. The recommendations follow logically from the findings.

There are no critical factual errors. There are several moderate issues around internal consistency (comparison table vs. detailed sections) and a few factual plausibility concerns. Minor issues are mostly editorial.

**Issues by severity:**
- Critical: 0
- Moderate: 7
- Minor: 8

---

## Critical Issues

None identified.

---

## Moderate Issues

### M1. Comparison table says Klaviyo has "No" control/holdback groups -- this is debatable

**Location:** Cross-Platform Patterns table, line 512

The table marks Klaviyo as "No" for control/holdback groups. However, the Klaviyo section (line 329) does not explicitly discuss holdback groups at all. Klaviyo's flow A/B testing works by splitting traffic between variants, and there is no mention of a dedicated "no-send" control group. The "No" rating is plausible but should be explicitly confirmed in the Klaviyo detailed section. As it stands, the Klaviyo section simply omits control group discussion, which creates an asymmetry -- every other competitor section addresses this.

**Recommended fix:** Add a sentence in the Klaviyo section under a "Control/Holdback Groups" heading, either confirming that Klaviyo does not support them or explaining the limitation. This closes the gap between the table and the detailed writeup.

---

### M2. Iterable "multiple variants" count is unspecified

**Location:** Cross-Platform Patterns table, line 514

The table shows Iterable as "Yes" for 3+ variants with no qualifier. However, the Iterable detailed section never specifies a maximum number of variants. It mentions "campaign variants" (line 351) and "A/B Split tiles" (line 327) but never states a limit. Every other platform in the table has a specific number (Braze: 8, SFMC: 10, Klaviyo: 2). The Iterable entry is inconsistent by omission.

**Recommended fix:** Either add the specific variant limit for Iterable in both the table and detailed section, or note "(limit not documented)" in the table cell.

---

### M3. Executive summary claim about Salesforce Email Studio statistical significance is incomplete

**Location:** Executive Summary, line 19

The executive summary states: "Salesforce Email Studio does not natively calculate statistical significance at all." This is accurate for Email Studio specifically, but could mislead readers about Salesforce Marketing Cloud as a whole. The detailed section clarifies that Path Optimizer also lacks documented statistical significance, and Einstein Content Testing uses a bandit algorithm with internal significance handling. The executive summary singles out "Email Studio" specifically but doesn't address SFMC more broadly.

**Recommended fix:** Rephrase to something like: "Salesforce Marketing Cloud does not natively calculate statistical significance in its Email Studio or Path Optimizer features."

---

### M4. Executive summary says Adobe "limits content experiments to campaigns" but detailed section is more nuanced

**Location:** Executive Summary, line 17 vs. Adobe section, lines 238-243

The executive summary says Adobe "limits content experiments to campaigns and does not yet fully support them in journeys." The detailed section (line 243) says: "Content experiments are primarily a campaigns feature. Full A/B testing within journeys is limited." and lists inbound channels (web, in-app, code-based experience) as "Available in any journey or campaign" (line 239). So content experiments ARE available in journeys for inbound channels. The executive summary overstates the limitation.

**Recommended fix:** Adjust the executive summary to: "Adobe Journey Optimizer supports content experiments in campaigns and for inbound channels in journeys, but does not fully support them for outbound channels (email, push, SMS) within journeys."

---

### M5. Comparison table marks Adobe Push A/B testing as "Yes" without qualification, but Adobe section says outbound channels require API-triggered campaigns

**Location:** Cross-Platform Patterns table, line 510 vs. Adobe section, line 240-241

The table says Adobe JO supports push A/B testing with a plain "Yes." But the Adobe detailed section states outbound channels (including push) are only "Supported in API-triggered transactional campaigns" (line 241). This is a meaningful constraint that deserves a qualifier, similar to how the table qualifies SMS as "Yes (API-triggered)." The table is inconsistent in qualifying SMS but not push for Adobe.

**Recommended fix:** Change Adobe's Push A/B testing cell from "Yes" to "Yes (API-triggered)" for consistency with the SMS cell in the same column.

---

### M6. Braze Experiment Paths limit: executive summary says "Experiment Paths" without stating the 4-path limit, but the comparison table shows "up to 8"

**Location:** Executive Summary, line 17 vs. Braze section, lines 75 and 128 vs. Cross-Platform table, line 514

The comparison table shows Braze has "Yes (up to 8)" for multiple variants. This correctly refers to campaign-level variants. However, the Experiment Paths (journey-level) are limited to 4 paths (line 75, line 128). The table does not distinguish between campaign variants (8) and journey path variants (4). For Braze, the "Multiple variants (3+)" row conflates two different features. Since the table row is about variants in general (not just journey paths), the "up to 8" label is technically correct but incomplete.

**Recommended fix:** Consider a footnote or qualifier like "Yes (up to 8 campaign / 4 journey)" or simply note this distinction somewhere near the table.

---

### M7. Adobe Journey Optimizer maximum variant count is never stated

**Location:** Cross-Platform Patterns table, line 514 and Adobe section

The table marks Adobe as "Yes" for 3+ variants with no upper limit stated. The Adobe detailed section also never specifies a maximum number of treatments. This is an information gap. Every other platform has a stated or implied limit.

**Recommended fix:** Either research and add Adobe's treatment limit, or note "(limit not documented)" in the table.

---

## Minor Issues

### m1. Inconsistent terminology: "holdback" vs. "holdout" vs. "control"

The report uses "control group" (Braze), "holdback group" (Salesforce), and "holdout group" (Adobe, Iterable) interchangeably. The comparison table uses "Control/holdback groups." This inconsistency reflects actual platform terminology but may confuse readers.

**Recommended fix:** Add a brief note in the Methodology section or as a footnote that these terms are used interchangeably across platforms, or standardize to one term with the platform-specific terms in parentheses.

---

### m2. Iterable section does not mention the number of A/B Split paths in journeys

The Braze section specifies 4 paths for Experiment Paths. The Salesforce section specifies up to 10 paths for Path Optimizer. The Iterable section says A/B Split tiles exist but never states how many branches are supported.

**Recommended fix:** Add the specific path limit if known.

---

### m3. Einstein Send Time Optimization described as having "experiment timeframes from 3 hours to 7 days" without further context

**Location:** Line 153

This detail appears under Salesforce's test types but is never referenced again in the statistical methodology or workflow sections. It's unclear whether this is truly an A/B test or more of a personalization feature.

**Recommended fix:** Briefly clarify whether Einstein STO is an experiment feature or an automated optimization feature, and whether it produces experiment-style results.

---

### m4. "Personalized Campaigns" profile threshold inconsistency

**Location:** Klaviyo section, lines 422 and 478

Line 422 says "Available for accounts with more than 400,000 total profiles." Line 478 says "requires 400,000+ profiles." These are consistent with each other, but it would be worth noting whether this is 400,000 contactable profiles or total profiles (including unsubscribed), as the threshold may differ in practice.

**Recommended fix:** Specify "total profiles" or "active/contactable profiles" if known.

---

### m5. Braze's logistic regression claim for Personalized Variants

**Location:** Braze section, line 89

The report states Braze uses "logistic regression to analyze user characteristics (recency, frequency, tenure)." This is plausible -- Braze documentation does reference these factors. However, the specific mention of logistic regression as the algorithm is worth flagging as a detail that may change or may be simplified in public documentation. This is not wrong, just worth noting as potentially fragile.

---

### m6. The "Gaps and Opportunities" section claim that no platform offers cross-channel A/B testing

**Location:** Line 599

The report says "While Iterable allows comparing email vs. SMS, most platforms require separate experiments for different channels." This partially contradicts Salesforce's Path Optimizer, which is described as supporting "email, SMS, push notifications, and advertising" across paths (line 155). Path Optimizer can theoretically compare different channels across different paths.

**Recommended fix:** Soften to: "While Iterable and Salesforce Path Optimizer allow some cross-channel comparison, a truly unified cross-channel experiment is not standard."

---

### m7. Recommendations section suggests "at least 4-5 testable paths" without clear justification

**Location:** Line 656

The recommendation for 4-5 paths seems to split the difference between Braze (4) and Salesforce (10). The rationale for this specific number is not stated.

**Recommended fix:** Add a brief rationale, e.g., "4-5 paths balances experiment complexity with actionability, covering the most common use cases."

---

### m8. Several sections mention Apple Mail Privacy Protection but no recommendation addresses it

**Location:** Lines 213, 484, 613

Two competitor sections and the Gaps section mention Apple Mail Privacy Protection inflating open rates. However, the Strategic Recommendations section does not address this. Since the report is meant to guide Infobip's implementation, it should address how to handle open rate unreliability.

**Recommended fix:** Add a brief recommendation about defaulting to click-based metrics or offering a toggle to exclude suspected machine opens, given the Apple MPP issue.

---

## Consistency Check Results

| Check | Result |
|---|---|
| Executive summary claims vs. detailed sections | Mostly consistent. Two moderate issues (M3, M4) where the summary oversimplifies. |
| Comparison table vs. individual sections | Several inconsistencies: Adobe push qualification missing (M5), Iterable variant limit unspecified (M2), Braze campaign vs. journey variant limits conflated (M6). |
| Cross-platform patterns vs. individual analyses | Consistent. Patterns are well-supported by the five individual analyses. |
| Recommendations vs. findings | Strong alignment. All recommendations trace back to specific findings. |
| Gaps/opportunities vs. competitor analyses | One minor inconsistency about cross-channel testing (m6). |
| Source lists: inline vs. end-of-report | The Sources section at the end is a superset of the per-section source links. End-of-report section includes additional references not in the per-section links (e.g., review sites, Trailhead modules, blog posts). No sources appear in per-section links that are missing from the final section. Consistent. |
| Terminology consistency | "Holdout" / "holdback" / "control" used interchangeably (m1). |

---

## Recommended Edits

Ordered by priority:

1. **Fix Adobe Push A/B testing qualifier in comparison table** (M5) -- change "Yes" to "Yes (API-triggered)" for consistency.

2. **Expand executive summary Adobe limitation** (M4) -- acknowledge that inbound channel experiments do work in journeys.

3. **Expand executive summary Salesforce limitation** (M3) -- include Path Optimizer alongside Email Studio in the "no significance" statement.

4. **Add control/holdback group discussion to Klaviyo section** (M1) -- explicitly address the gap.

5. **Specify or acknowledge unknown variant limits** for Iterable (M2) and Adobe (M7) in both the table and detailed sections.

6. **Add Braze campaign vs. journey variant limit distinction** (M6) -- clarify in the table or add a footnote.

7. **Soften the cross-channel testing gap claim** (m6) -- acknowledge Salesforce Path Optimizer alongside Iterable.

8. **Add an Apple Mail Privacy Protection recommendation** (m8) -- advise defaulting to click-based metrics.

9. **Standardize holdout/holdback/control terminology** (m1) -- add a terminology note.

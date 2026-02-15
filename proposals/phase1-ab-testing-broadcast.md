# Phase 1: A/B Testing for Broadcast Campaigns

**Date:** February 15, 2026
**Status:** Proposal
**Audience:** Product team, engineering leads, stakeholders

---

## 1. Summary

We propose adding A/B testing to broadcast campaigns in Infobip Moments. In Phase 1, marketers will be able to create two message variants (A and B) for any broadcast campaign, split their audience, and compare which variant performs better. Two modes will be available: an equal 50/50 split across the full audience, and a "test-then-send-winner" mode that tests on a small group first and sends the best-performing variant to the rest. This feature closes a competitive gap — every major platform in our space already offers A/B testing — and gives our customers a data-driven way to improve their campaign results.

---

## 2. Problem Statement

### Why broadcast campaigns need A/B testing

Today, when a marketer creates a broadcast campaign in Moments, they write one message and send it to their entire audience. They have no way to compare two different approaches and learn which one works better. This means:

- **Guessing instead of testing.** Marketers rely on intuition or past experience to decide what subject line, body text, or call-to-action to use. There is no built-in way to let the data decide.
- **Missed optimization.** Small changes in wording, images, or CTAs can have a big impact on open rates and click rates. Without A/B testing, these improvements stay undiscovered.
- **Competitive disadvantage.** Our competitive analysis shows that all five major competitors (Braze, Salesforce Marketing Cloud, Adobe Journey Optimizer, Iterable, and Klaviyo) offer A/B testing as a core feature. It is considered a basic expectation in this market. Customers evaluating platforms will notice this gap.

A/B testing is the most common method for marketers to make better decisions based on real data. Adding it to broadcast campaigns is a necessary step to keep Moments competitive and to help our customers get better results from their campaigns.

---

## 3. Proposed Solution

### 3.1 Overview of the Two Testing Modes

Phase 1 offers two ways to run an A/B test:

| | Equal Split (100%) | Test-Then-Send-Winner |
|---|---|---|
| **How it works** | The entire audience is split 50/50. Half gets Variant A, half gets Variant B. | A small test group (e.g., 20%) gets the variants. The winner is sent to the remaining audience (e.g., 80%). |
| **Who picks the winner** | The marketer reviews results and draws conclusions manually. | The system picks the winner automatically based on a metric the marketer chose before launch. |
| **Best for** | Learning and comparison. The marketer wants to understand how two approaches perform across the full audience. | Optimization. The marketer wants the best-performing message to reach most of the audience. |
| **Audience size concern** | Works with any audience size, but very small audiences may not produce meaningful differences. | Needs a large enough test group to produce reliable results. The system will show a warning if the test group is too small. |

### 3.2 Where A/B Testing Fits in the Broadcast Flow

A/B testing adds a new step to the existing broadcast creation flow. It is introduced early — right after channel selection — so that the marketer works with two variants throughout the rest of the process.

| Step | Current Flow | New Flow with A/B Testing |
|---|---|---|
| 1 | Channel selection | Channel selection *(no change)* |
| **New** | — | **A/B test setup** — Toggle A/B test on/off. If on: choose testing mode (equal split or test-then-send-winner). For test-then-send-winner: set test group size (%) and wait period. |
| 2 | Recipients | Recipients *(no change — same audience for both variants)* |
| 3 | Message composition | **Message composition for Variant A and Variant B** — The composer shows two tabs or a side-by-side view. The marketer edits each variant separately. |
| 4 | Settings | Settings *(mostly no change)*. For test-then-send-winner mode: the marketer picks the winning metric (open rate, click rate, or delivery rate). |
| 5 | Preview & Launch | **Preview & Launch** — Shows both variants for review. Summary includes: testing mode, split percentages, winning metric (if applicable), and wait period (if applicable). |

**Key design principle:** Both variants share the same channel, same recipients, same sender, and same schedule. Only the message content differs.

### 3.3 Variant Creation and Content Editing

When A/B testing is turned on:

- The message composition step shows **two content areas** — one for Variant A and one for Variant B.
- The marketer can edit each variant independently: different subject lines (email), different body text, different images, different CTAs.
- A **"Copy from A"** button lets the marketer duplicate Variant A into Variant B and then make small changes (useful for subject line tests where only one element differs).
- The existing **"Send test"** button works for each variant separately, so the marketer can preview both before launch.
- The live device preview shows each variant.
- Both variants can be saved as templates independently.

**What can differ between variants:**

| Element | Can differ? |
|---|---|
| Subject line (email) | Yes |
| Body text | Yes |
| Images | Yes |
| Call-to-action (button/link) | Yes |
| Sender | No (same for both) |
| Channel | No (same for both) |
| Schedule | No (same for both) |
| Recipients | No (same audience, split by the system) |

### 3.4 Audience Splitting Logic

**Equal split mode:**
- The system divides the audience randomly into two groups of equal size (50/50).
- Both groups receive the campaign at the same time, on the same schedule.

**Test-then-send-winner mode:**
- The marketer sets the **test group size** as a percentage (e.g., 20%).
- The test group is split evenly between Variant A and Variant B (e.g., 10% gets A, 10% gets B).
- The remaining audience (e.g., 80%) waits.
- After the wait period, the system sends the winning variant to the remaining 80%.

**Example with 100,000 recipients and a 20% test group:**

| Group | Size | What they receive | When |
|---|---|---|---|
| Test Group A | 10,000 (10%) | Variant A | At scheduled send time |
| Test Group B | 10,000 (10%) | Variant B | At scheduled send time |
| Remaining | 80,000 (80%) | Winning variant | After wait period ends |

The marketer can set the test group size between 10% and 50%. The system will show a recommendation based on audience size (larger audiences can use smaller test groups).

### 3.5 Winner Selection Mechanism

Winner selection applies only to the **test-then-send-winner** mode.

**Before launch**, the marketer picks one winning metric:

| Metric | Available for | Best for |
|---|---|---|
| Open rate | Email only | Testing subject lines, preheaders |
| Click rate | All channels with links | Testing body content, CTAs, images |
| Delivery rate | All channels | Testing content that may affect deliverability (e.g., SMS in markets with content filtering) |

**After the wait period ends**, the system compares the two variants on the chosen metric:

1. **Clear winner exists** — The variant with the better metric value is sent to the remaining audience automatically.
2. **Results are tied or not enough data** — The system notifies the marketer and offers two choices:
   - Pick a winner manually.
   - If no action is taken within a configurable timeout (default: 2 hours), Variant A is sent as the default.

**Wait period configuration:**
- The marketer sets the wait period before launch (e.g., 1 hour, 4 hours, 24 hours).
- The system shows a recommendation based on the channel and winning metric. For example: "For email open rate tests, we recommend waiting at least 4 hours."

### 3.6 Results and Analytics

A/B test campaigns get a dedicated analytics view that shows both variants side by side.

**Side-by-side comparison table:**

| Metric | Variant A | Variant B | Better |
|---|---|---|---|
| Sent | 10,000 | 10,000 | — |
| Delivered | 9,800 | 9,750 | A |
| Delivery rate | 98.0% | 97.5% | A |
| Opens (email) | 2,450 | 2,800 | B |
| Open rate (email) | 25.0% | 28.7% | **B** |
| Clicks | 490 | 620 | B |
| Click rate | 5.0% | 6.4% | **B** |

**Statistical confidence indicator:**

Instead of showing raw statistical numbers, we use plain-language labels:

| Label | Meaning | When shown |
|---|---|---|
| **Confident** | The difference between variants is large enough to be reliable. You can trust this result. | Statistical confidence is 95% or higher. |
| **Promising** | One variant looks better, but the difference might be due to chance. Consider testing with a larger audience. | Statistical confidence is between 80% and 95%. |
| **Not enough data** | The audience is too small or the results are too close to draw a conclusion. | Statistical confidence is below 80%. |

**Additional analytics features:**
- Highlight the winning metric with a visual marker (e.g., green badge on the better variant).
- For test-then-send-winner campaigns: show results for the test phase and the final send phase separately.
- All existing analytics features (segmentation by country, operator, delivery status) work for each variant independently.
- The campaign analytics page clearly labels the campaign as an "A/B Test" with the testing mode used.

---

## 4. User Workflow

### 4.1 Equal Split Mode — User Story

> *As a marketer, I want to send two different versions of my email to my entire audience, split 50/50, so I can compare which version gets more clicks and use that learning for future campaigns.*

**Step-by-step:**

1. I start creating a new broadcast campaign and pick **Email** as my channel.
2. On the next screen, I see a toggle: **"Enable A/B Test."** I turn it on.
3. I select **"Equal split"** mode. The screen shows: "Your audience will be split 50/50 between Variant A and Variant B."
4. I set up my recipients (upload a file of 50,000 email addresses).
5. The message composer opens with **two tabs: Variant A and Variant B**.
6. I write my first email in the Variant A tab — subject line: "Spring Sale: 30% Off Everything."
7. I click **"Copy from A"** in the Variant B tab. The system duplicates my email.
8. I change only the subject line in Variant B: "Your Exclusive Spring Discount Inside."
9. I use **"Send test"** on each tab to preview both emails in my inbox.
10. In Settings, I set the schedule for tomorrow at 10:00 AM. I enable click tracking.
11. On the Preview & Launch screen, I see a summary:
    - A/B test: Equal split (50/50)
    - Variant A subject: "Spring Sale: 30% Off Everything"
    - Variant B subject: "Your Exclusive Spring Discount Inside"
    - Audience: 50,000 (25,000 per variant)
    - Schedule: Tomorrow, 10:00 AM
12. I name the campaign "Spring Sale A/B Test" and hit **Launch**.
13. The next day, I check the analytics. I see both variants side by side. Variant B has a 6.2% click rate vs. Variant A's 4.8%. The confidence label says **"Confident."** I now know that the curiosity-driven subject line works better and will use similar wording in the future.

### 4.2 Test-Then-Send-Winner Mode — User Story

> *As a marketer, I want to test two SMS messages with a small group first and automatically send the better one to the rest of my audience, so I get the best results without manually monitoring the test.*

**Step-by-step:**

1. I start creating a new broadcast campaign and pick **SMS** as my channel.
2. I turn on **"Enable A/B Test"** and select **"Test, then send winner"** mode.
3. I set the **test group size to 20%** and the **wait period to 2 hours**.
4. I pick **Click rate** as the winning metric.
5. I set up my recipients (a People segment of 200,000 contacts).
6. The message composer shows two tabs. I write two different SMS messages:
   - Variant A: "Hi {firstName}, flash sale today! 40% off with code FLASH40. Shop now: {link}"
   - Variant B: "Hey {firstName}, only 6 hours left! Use code SAVE40 for 40% off: {link}"
7. I use "Send test" to verify both messages look correct.
8. In Settings, I schedule the campaign for today at 2:00 PM. I enable URL shortening and click tracking.
9. On the Preview & Launch screen, I see:
   - A/B test: Test, then send winner
   - Test group: 20% (40,000 contacts — 20,000 per variant)
   - Remaining audience: 80% (160,000 contacts)
   - Winning metric: Click rate
   - Wait period: 2 hours
   - Winner send: approximately 4:00 PM
10. I name the campaign "Flash Sale SMS A/B" and hit **Launch**.
11. At 2:00 PM, the system sends Variant A to 20,000 contacts and Variant B to 20,000 contacts.
12. At 4:00 PM, the system checks click rates. Variant B has 3.1% click rate vs. Variant A's 2.4%. The difference is clear.
13. The system automatically sends **Variant B** to the remaining 160,000 contacts.
14. I receive a notification: "A/B test complete. Variant B won with 3.1% click rate (vs. 2.4% for A). Variant B was sent to the remaining 160,000 recipients."
15. In analytics, I see: test phase results (side by side) and final send results (Variant B performance on the full remaining audience).

### 4.3 Tied Results — User Story

> *What happens when neither variant is clearly better?*

1. I launched a test-then-send-winner campaign with a 2-hour wait period.
2. After 2 hours, Variant A has 2.5% click rate and Variant B has 2.6%. The difference is too small to be meaningful.
3. The system shows the status: **"Results are not clear enough to pick a winner automatically."**
4. I receive a notification asking me to **pick a winner manually**.
5. I review the results and pick Variant B because, even though the difference is small, the message aligns better with our brand tone.
6. The system sends Variant B to the remaining audience.
7. **If I do nothing:** After 2 hours (the default timeout), the system sends Variant A as the default and notifies me.

---

## 5. Technical Considerations

This section describes the high-level technical direction. Detailed design will follow in a separate engineering document.

### 5.1 Server-Side Variant Assignment

- All variant assignment happens on the server side at send time. This is the industry standard (all competitors use this approach).
- When the campaign is triggered, the system assigns each recipient to either Variant A or Variant B before sending.
- The assignment is stored so that analytics can attribute delivery and engagement events to the correct variant.

### 5.2 Audience Split Randomization

- Recipients must be split randomly to avoid bias. For example, the system should NOT assign the first half of the list to A and the second half to B (lists may be sorted by signup date, geography, etc.).
- A good approach: use a hash of the recipient identifier (e.g., phone number or email) combined with the campaign ID to assign variants. This is deterministic (same input always gives the same result) and produces an even distribution.
- The randomization must maintain the configured split ratio (50/50 for equal split; the configured percentage for test-then-send-winner).

### 5.3 Winner Determination Timing and Logic

- For test-then-send-winner mode, the system needs a scheduled job that runs after the wait period ends.
- At that point, it queries the Metrics API for the selected metric (open rate, click rate, or delivery rate) for each variant.
- It compares the values and decides:
  - **Clear winner:** Proceed automatically.
  - **No clear winner:** Notify the marketer and wait for manual selection (with a timeout that defaults to Variant A).
- The winner determination should account for statistical significance. A simple approach: use a two-proportion z-test to compare rates. The result maps to the confidence labels ("Confident" at 95%+, "Promising" at 80-95%, "Not enough data" below 80%).
- Edge case: if one variant has a significantly higher delivery failure rate, fewer recipients may have received it, which affects the comparison. The system should compare rates (not raw counts) to handle this.

### 5.4 Analytics Data Model Changes

- Each broadcast campaign with A/B testing enabled needs to store:
  - The testing mode (equal split or test-then-send-winner).
  - The variant assignment for each recipient (A or B).
  - The winning metric (for test-then-send-winner).
  - The winner (A or B) and how it was determined (automatic or manual).
  - The wait period and test group size (for test-then-send-winner).
- Engagement events (delivered, opened, clicked) must be linked to the correct variant.
- The analytics query layer must support filtering and grouping by variant.

### 5.5 Impact on Existing Metrics API

- The Metrics API currently returns campaign-level aggregates. It needs to support a new dimension: **variant**.
- Queries like "get delivery rate for campaign X, variant A" must be possible.
- The existing campaign-level aggregates should still work as before (totals across both variants) to avoid breaking current integrations.
- The side-by-side comparison view is a new analytics component that reads from the same Metrics API with variant-level filtering.

---

## 6. What's NOT in Phase 1

These items are intentionally out of scope for Phase 1. Each has a brief explanation of why.

| Out of scope | Reason |
|---|---|
| **More than 2 variants** | Two variants cover the most common A/B testing use case (and what Salesforce and Klaviyo recommend). Adding more variants increases complexity in the UI, the splitting logic, and the statistical analysis. We can add this in a later phase. |
| **Control/holdback group** (a group that receives no message) | Useful for measuring "did sending this campaign matter at all?" but adds complexity and is a less common need for broadcast. Can be added as an optional setting in a future phase. |
| **Journey-level A/B testing** | This proposal covers broadcast (one-time campaigns) only. Journey/flow testing is a separate, larger initiative. |
| **API support for A/B test creation** | Phase 1 is UI-only. API endpoints for creating and managing A/B tests will come in a later phase. (Note: analytics data for A/B campaigns will be accessible via the Metrics API once variant-level filtering is added.) |
| **Multi-armed bandit / automatic traffic shifting** | Advanced optimization that shifts more traffic to the better variant during the test. Adds significant complexity. Better suited for journey/triggered campaigns than one-time broadcasts. |
| **AI-powered personalized variants** | Sending each recipient the variant predicted to work best for them. Requires ML infrastructure. A longer-term initiative. |
| **Custom conversion events as a winning metric** | Phase 1 winning metrics are limited to open rate, click rate, and delivery rate. Custom events (e.g., purchases) require conversion tracking that does not exist in broadcast analytics today. |
| **Cross-channel testing** (e.g., SMS vs. Email) | Phase 1 requires both variants to use the same channel. Cross-channel comparison is a different type of experiment. |
| **Different senders per variant** | Both variants must use the same sender. Testing different senders adds regulatory and deliverability complexity. |

---

## 7. Open Questions

These questions need input from the team before we finalize the design.

| # | Question | Context |
|---|---|---|
| 1 | **What should the minimum test group size be for test-then-send-winner mode?** We propose 10% as the floor, but should we set a minimum absolute number (e.g., at least 1,000 per variant) to avoid meaningless results? | Small test groups produce unreliable statistical results. But setting the floor too high may block users with smaller audiences from using this mode. |
| 2 | **Where exactly in the UI should the A/B test toggle live?** We proposed it as a new step after channel selection. An alternative: place it inside the message composition step (like Klaviyo's "Create A/B test" button). Which fits better with the current design system? | The "new step" approach makes A/B testing more visible. The "inside message composition" approach is simpler but may be easy to miss. Needs input from the design team. |
| 3 | **How should the Campaign Approval Manager role interact with A/B tests?** If a Content Creator sets up both variants, does the Approval Manager review and approve each variant separately, or approve the campaign as a whole? | The two-role workflow (creator + approver) is an existing pattern. We need to decide if A/B testing changes the approval process. |
| 4 | **Should the default timeout for manual winner selection be configurable?** We proposed a 2-hour default (if the marketer doesn't pick a winner manually when results are tied, Variant A is sent). Should this be user-configurable, or is a fixed default enough for Phase 1? | Making it configurable adds UI complexity. A fixed default is simpler but may not fit all use cases. |
| 5 | **How do we handle Send-Time Optimization (STO) with A/B testing?** STO sends messages at the best time for each individual recipient. If STO is enabled, recipients in the test group will receive messages at different times, which makes the wait period harder to define. Should STO and A/B testing be mutually exclusive in Phase 1? | STO and A/B testing interact in complex ways. If a recipient in the test group gets their message at 6 PM because of STO, but the wait period ends at 4 PM, their engagement data is not included in the winner decision. |
| 6 | **Do we need to show a sample size recommendation before launch?** For example: "Based on your audience of 50,000, we recommend a test group of at least 10% (5,000) and a wait period of at least 4 hours for reliable results." | This would improve the user experience, but requires defining the logic for recommendations. We could launch without it and add it later, or include it from the start. |
| 7 | **Should the "equal split" mode also show the statistical confidence label?** Or is the confidence label only relevant for test-then-send-winner (where an automated decision depends on it)? | In equal split mode, the marketer makes their own conclusions. Showing confidence helps them understand if the difference is real, but it might also confuse users who just want a simple comparison. |

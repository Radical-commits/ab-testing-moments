<objective>
Write a solution proposal for Phase 1 of A/B testing in Infobip Moments broadcast campaigns.

This proposal will be shared with the product team, engineering leads, and stakeholders. It should clearly explain what we're building, why, and how — in plain language accessible to non-native English speakers.
</objective>

<context>
Infobip Moments is a customer engagement platform. Broadcast is the feature for sending one-time campaigns to a list of recipients. Today, broadcast has NO A/B testing capability.

Read the competitive analysis for full market context:
@research/ab-testing-competitive-analysis.md

<current_broadcast_flow>
The current broadcast creation flow has these steps:
1. **Channel selection** — Pick one channel: SMS, Email, MMS, Push, RCS, Viber, Voice, WhatsApp, Zalo
2. **Recipients** — Add audience via People profiles, file upload, or existing files. Map fields (phone/email). Supports personalization placeholders.
3. **Message composition** — Select sender, write content, channel-specific formatting, live device preview. Can save as template.
4. **Settings** — Scheduling (date/time, time zone, delivery window, Send-Time Optimization), tracking (URL shortening, open tracking for email, click tracking), delivery speed, validity period, failover SMS config.
5. **Preview & Launch** — Name the draft, review all settings, hit "Launch."
6. **"Send test"** button exists for single-message validation (Email, RCS, SMS, Zalo).
</current_broadcast_flow>

<current_analytics>
Post-launch analytics include:
- Delivery metrics: sent, pending, delivered, delivery rate
- Engagement: opens/seen, total clicks, unique clicks, CTR, clicked URLs
- Email-specific: unsubscribes, spam complaints
- Segmentation: by country, operator, channel (failover), delivery status
- No conversion event tracking beyond URL clicks
- Data retained 12 months (detailed), summary forever
- Powered by Metrics API
</current_analytics>

<user_roles>
Two campaign roles exist:
- Campaign Content Creator (creates content and audience)
- Campaign Approval Manager (reviews and launches)
</user_roles>
</context>

<requirements>
Phase 1 scope — keep it focused:

1. **2 content variants (A and B)** for any single broadcast campaign
   - Each variant can have different message content (subject line, body, images, CTAs)
   - Both variants use the same channel, same recipients, same sender, same schedule

2. **Two testing modes:**
   - **Equal split (100%)** — Split the entire audience 50/50 between A and B. Marketer reviews results manually and draws conclusions.
   - **Test-then-send-winner** — Send variants to a configurable test subset (e.g., 20% of audience, split evenly). After a configurable wait period, automatically send the winning variant to the remaining 80%.

3. **Winner selection criteria:**
   - For test-then-send-winner mode, the marketer picks one winning metric before launch:
     - Open rate (email only)
     - Click rate
     - Delivery rate (useful for channels where deliverability varies)
   - If results are tied or inconclusive, the marketer can choose manually, or a default (Variant A) is sent.

4. **Analytics for A/B campaigns:**
   - Side-by-side comparison of Variant A vs B metrics
   - Clear indication of which variant performed better
   - Statistical confidence indicator (plain-language: "Confident", "Promising", "Not enough data")
   - Works within the existing analytics framework (Metrics API)

5. **Control/holdback group:** NOT in Phase 1 scope (mention as future enhancement)

6. **API support:** NOT in Phase 1 scope (mention as future enhancement)
</requirements>

<proposal_structure>
Write the proposal with these sections:

1. **Summary** (3-4 sentences: what, why, expected impact)
2. **Problem statement** (why broadcast campaigns need A/B testing — reference competitive landscape briefly)
3. **Proposed solution**
   - Overview of the two testing modes
   - How it fits into the existing broadcast flow (where in the steps does A/B get configured?)
   - Variant creation and content editing
   - Audience splitting logic
   - Winner selection mechanism
   - Results and analytics
4. **User workflow** (step-by-step for each mode, written as a user story)
5. **Technical considerations** (high-level, not implementation details)
   - Server-side variant assignment
   - Audience split randomization
   - Winner determination timing and logic
   - Analytics data model changes
   - Impact on existing Metrics API
6. **What's NOT in Phase 1** (explicit scope boundaries with brief rationale)
7. **Open questions** (things that need team input to decide)

Use tables and bullet points for clarity. Avoid jargon. Keep sentences short.
</proposal_structure>

<output>
Save the proposal to: `./proposals/phase1-ab-testing-broadcast.md`
</output>

<verification>
Before finishing, verify:
- Every requirement from the requirements section is addressed in the proposal
- The user workflow sections are concrete enough that a designer could mock them up
- Technical considerations are high-level (no code, no API specs — just architectural direction)
- Open questions are genuinely open (not things already decided)
- Language is simple and accessible to non-native English speakers
</verification>

<success_criteria>
- The proposal reads as a self-contained document that someone unfamiliar with this project could understand
- Both testing modes (equal split and test-then-send-winner) are clearly described
- The proposal clearly shows WHERE in the existing broadcast flow A/B testing fits
- Open questions section has at least 3 genuine questions for team discussion
</success_criteria>

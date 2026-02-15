# A/B Testing Competitive Analysis for Infobip Moments

**Prepared for:** Infobip Moments Product Team, Engineering Leads, and Executive Leadership
**Research Date:** February 12, 2026
**Author:** Competitive Research Team

---

## Executive Summary

This report analyzes how five major marketing automation and customer engagement platforms handle A/B testing. The platforms studied are **Braze**, **Salesforce Marketing Cloud**, **Adobe Journey Optimizer**, **Iterable**, and **Klaviyo**. Together, they represent the most relevant competitors to Infobip Moments in terms of feature scope and market positioning.

### Key Findings

1. **All five platforms support A/B testing as a core feature**, but the depth, sophistication, and ease of use vary widely. A/B testing is considered table stakes in this market.

2. **Journey-level (path) testing is becoming the new standard.** Braze (Experiment Paths), Salesforce (Path Optimizer), and Iterable (A/B Split tiles) all allow testing of entire journey branches, not just individual message content. Adobe Journey Optimizer supports content experiments in campaigns and for inbound channels (web, in-app) in journeys, but does not fully support them for outbound channels (email, push, SMS) within journeys.

3. **Automatic winner selection with statistical methods is universal**, but approaches differ. Braze uses Pearson's chi-squared tests at 95% confidence. Adobe uses advanced "anytime valid" confidence sequences that allow safe early peeking at results. Klaviyo uses a simpler win probability model with a 90% threshold. Salesforce Marketing Cloud does not natively calculate statistical significance in its Email Studio or Path Optimizer features.

4. **AI-powered personalization of test variants is an emerging differentiator.** Braze's "Personalized Variants/Paths" and Klaviyo's "Personalized Campaigns" use machine learning to send each user the variant most likely to work for them, rather than just picking one global winner. Adobe's new Experimentation Accelerator (separate license) uses agentic AI to analyze results and suggest new experiments.

5. **Multi-armed bandit testing is gaining ground.** Adobe Journey Optimizer and Iterable both offer multi-armed bandit algorithms that automatically shift traffic toward better-performing variants during the test, rather than waiting for a fixed test period to end.

### High-Level Recommendations

- Implement journey-level A/B testing (path testing) as a core capability, since this is where the market is heading.
- Offer both traditional A/B testing with fixed splits and automatic winner selection, as well as a multi-armed bandit option for continuous optimization.
- Invest in AI-powered personalized variant selection as a mid-term differentiator.
- Use clear, built-in statistical significance indicators (confidence levels) so marketers can trust their results without needing external tools.

---

## Methodology

### Competitors Analyzed

| Competitor | Why Selected |
|---|---|
| **Braze** | Direct competitor in mobile-first customer engagement; strong in push, email, and in-app messaging |
| **Salesforce Marketing Cloud** | Market leader in enterprise marketing automation; widely adopted across industries |
| **Adobe Journey Optimizer** | Enterprise CX orchestration platform; strong in omnichannel journey management |
| **Iterable** | Growth-stage competitor focused on cross-channel lifecycle marketing |
| **Klaviyo** | Strong in e-commerce and SMB/mid-market; rapidly growing in email and SMS marketing |

### Research Sources

Information was gathered from:
- Official product documentation and help centers (primary source)
- Product marketing pages and feature descriptions
- Technical blog posts from the companies
- User reviews on G2, Capterra, Gartner Peer Insights, and TrustRadius
- Community forums and support threads

### Important Notes

- Product capabilities change frequently. All information reflects what was publicly available as of February 2026.
- Where specific details could not be confirmed through official sources, this is noted explicitly.
- Pricing details are generally not publicly available for enterprise platforms and are excluded.

### Terminology Note

Different platforms use different terms for the same concept of withholding messages from a portion of the test audience to measure baseline impact. Braze calls it a "control group," Salesforce uses "holdback group," and Adobe and Iterable use "holdout group." These terms are used interchangeably in this report, matching each platform's own terminology in the detailed sections.

---

## Competitor Analysis

---

### 1. Braze

**Overview:** Braze is a customer engagement platform focused on real-time, cross-channel messaging. It serves mid-market and enterprise customers, with particular strength in mobile-first engagement (push notifications, in-app messages) alongside email, SMS, and webhooks.

#### A/B Testing Implementation

**Test Types and Scope:**
- **Campaign-level A/B testing:** Up to 8 message variants can be tested per campaign. Each variant can differ in content, subject line, images, or other elements. A/B tests (one variable changed) and multivariate tests (multiple variables changed) are both supported.
- **Journey-level testing (Canvas Experiment Paths):** Within Braze's Canvas (journey builder), Experiment Paths allow testing of entire journey branches. Up to 4 paths can be compared, each containing different sequences of messages, timing, or channel combinations.
- **Feature Flag A/B testing:** Braze also supports A/B testing through Feature Flags, allowing product and engineering teams to test new app/website features with specific user groups.

**Supported Channels:** Email, push notifications, in-app messages, SMS, WhatsApp, and webhooks.

**Technical Approach:**
- Server-side execution. Variant assignment happens on Braze's servers when the campaign or Canvas is triggered.
- Each user is randomly assigned to a variant or control group at send time. The assignment is persistent for re-eligible campaigns (the same user gets the same path if they re-enter).
- Campaigns must target a single channel and device type (e.g., iOS push only, not iOS + Android in the same test).

**Control Groups:** Supported at both campaign and Canvas level. A percentage of the audience can be held back to measure the incremental effect of any messaging versus no messaging. Braze warns that control groups should not be used when the winning metric is opens or clicks, since control group members receive no message.

**Optimization Features:**
- **Winning Variant:** After an initial test period, the best-performing variant is automatically sent to the remaining reserved audience. Available for push, email, webhook, SMS, and WhatsApp campaigns with single-send scheduling.
- **Personalized Variants/Paths:** Uses logistic regression to analyze user characteristics (recency, frequency, tenure) and predict which variant each individual user would respond to best. If no meaningful user-to-variant associations are found, it falls back to the Winning Variant approach.

**API Access:** A/B tests are primarily configured through the Braze dashboard. The REST API supports triggering campaigns and exporting analytics, but there are no dedicated API endpoints for creating or managing A/B test configurations programmatically.

#### User Experience and Workflow

**Setup Process:**
1. Create a campaign and select a single channel.
2. Compose up to 8 message variants.
3. Schedule the campaign (standard, action-based, or API-triggered delivery).
4. Select target segments and distribute audience across variants and optional control group.
5. Optionally configure Winning Variant or Personalized Variants optimization.
6. Review and launch.

**Complexity Level:** Moderate. The interface is powerful but has a learning curve. G2 reviewers note a lower Ease of Setup rating (7.5/10) compared to some competitors. Most teams need engineering support for initial setup.

**Visual Configuration:** Variants are composed side-by-side in the campaign builder. For Canvas Experiment Paths, a visual drag-and-drop editor shows branching paths clearly.

**Results Presentation:** Analytics are shown in two tabs: "Initial Test" (performance during the test phase) and "Winning/Personalized Variant" (results from the final send). Braze also calculates "projected lift" from personalization.

#### Statistical Methodology and Analytics

- **Method:** Pearson's chi-squared test with Z-test comparisons between each variant's conversion rate and the control group.
- **Significance Threshold:** 95% confidence (p < 0.05).
- **Confidence Display:** A 0-100% confidence score is shown for each variant comparison. Results below 95% are flagged as not statistically significant.
- **Winner Determination:** A variant is labeled "Winner" only if it statistically outperforms all others at 95% confidence. If no variant reaches this threshold, the best-performing variant is still sent but flagged as not statistically proven.
- **Metrics Available:** Unique opens, clicks, bounces, primary conversion events, and channel-specific engagement data.
- **Sample Size Guidance:** Braze recommends approximately 15,000 users per variant for statistically significant results, though actual requirements vary.
- **Early Stopping:** Braze explicitly warns against ending tests early, as this can bias results.

#### Notable Strengths
- Journey-level Experiment Paths with up to 4 paths
- Personalized Variants using ML to match users to their best variant
- Feature Flag A/B testing for product experiments
- Clear statistical methodology with 95% confidence threshold

#### Notable Limitations
- Campaign A/B tests limited to single channel and single device type
- No mid-campaign modifications allowed (experiment is disabled if changes are made)
- Experiment Paths limited to 4 paths (vs. 8 variants in campaigns)
- Optimizations not available for recurring or re-eligible campaigns
- Steep learning curve and requires technical expertise for setup

#### Source Links
- [Multivariate and A/B Testing Overview](https://www.braze.com/docs/user_guide/engagement_tools/testing/multivariant_testing)
- [Create Multivariate and A/B Tests](https://www.braze.com/docs/user_guide/engagement_tools/testing/multivariant_testing/create_multivariate_campaign)
- [Multivariate and A/B Test Analytics](https://www.braze.com/docs/user_guide/engagement_tools/testing/multivariant_testing/multivariate_analytics)
- [Optimizations (Winning Variant & Personalized Variants)](https://www.braze.com/docs/user_guide/engagement_tools/testing/multivariant_testing/optimizations)
- [Canvas Experiment Paths](https://www.braze.com/docs/user_guide/engagement_tools/canvas/canvas_components/experiment_step)
- [Personalized Paths in Canvas](https://www.braze.com/docs/user_guide/engagement_tools/canvas/canvas_components/experiment_step/personalized_paths)
- [Feature Flag A/B Testing](https://www.braze.com/resources/articles/building-braze-feature-flags-ab-testing)

---

### 2. Salesforce Marketing Cloud

**Overview:** Salesforce Marketing Cloud is an enterprise marketing automation platform with a broad feature set spanning email marketing (Email Studio), journey orchestration (Journey Builder), advertising, social media, and AI-powered personalization (Einstein). It is one of the most widely deployed marketing platforms globally.

#### A/B Testing Implementation

**Test Types and Scope:**
- **Email Studio A/B Testing:** Traditional A/B testing for single-send emails. Tests can compare subject lines, email content, content areas, from name, send time, and preheaders. Limited to two variants (A vs. B).
- **Path Optimizer (Journey Builder):** A more advanced testing capability that allows up to 10 path variants within a journey. Each path can contain different messages, channels, timing, or content. This is the primary A/B/n testing feature for journey-level experiments.
- **Einstein Content Testing:** AI-powered testing that automatically selects the best-performing image or content block from a set, optimizing in real-time using a multi-armed bandit approach. Available in Professional, Corporate, and Enterprise editions.
- **Einstein Send Time Optimization:** An automated optimization feature (not a traditional A/B experiment) that tests and optimizes send times based on past engagement data, with experiment timeframes from 3 hours to 7 days. It does not produce traditional experiment-style results with variant comparison.

**Supported Channels:** Email (primary for A/B testing), SMS, push notifications, and advertising (via Path Optimizer in Journey Builder).

**Technical Approach:**
- Server-side execution for all test types.
- Email Studio A/B tests send to a configurable percentage of the audience, then the winner is sent to the remainder.
- Path Optimizer randomly distributes contacts across up to 10 paths with configurable percentages.
- Einstein Content Testing uses real-time optimization, selecting the best content at the moment each email is opened.

**Control/Holdback Groups:** Path Optimizer supports holdback groups (available only for "Run Once" journeys). Email Studio A/B tests do not have a native holdback feature.

**API Access:** Salesforce Marketing Cloud has extensive REST and SOAP APIs for campaign management, journey operations, and data extensions. However, dedicated A/B test creation endpoints are not prominently featured. Most A/B test setup is done through the UI. Programmatic testing workflows typically combine the Journey Builder API with Path Optimizer or use AMPscript/SSJS within emails.

#### User Experience and Workflow

**Email Studio A/B Test Setup:**
1. Click "Create A/B Test" and provide name and description.
2. Select the test type (subject line, content, from name, send time, etc.).
3. Choose the audience (subscriber lists, groups, or data extensions).
4. Set the distribution percentage between test and remainder.
5. Define winner criteria (highest unique open rate or click-through rate).
6. Set test duration (typically 24-48 hours recommended).
7. Launch the test.

**Path Optimizer Setup:**
1. Drag the Path Optimizer activity into a journey in Journey Builder.
2. Configure 2-10 paths with distribution percentages.
3. Add different messages, channels, or timing to each path.
4. Choose winner selection method: automatic (based on opens, clicks, or unsubscribes) or manual.
5. Set test duration (accounting for any wait periods in paths).
6. Activate the journey.

**Complexity Level:** Moderate to high. The split between Email Studio and Journey Builder creates confusion, as A/B testing works differently in each context. Path Optimizer is more powerful but requires Journey Builder knowledge. Einstein features add another layer of complexity.

**Results Presentation:** Path Optimizer shows a summary screen and post-test performance view. Email Studio shows comparative metrics between variants. Einstein Content Testing provides real-time performance dashboards per content asset.

#### Statistical Methodology and Analytics

- **Email Studio:** Does NOT natively calculate statistical significance. It simply picks the winner based on which version has a higher metric value. Salesforce recommends using external statistical significance calculators to verify results.
- **Path Optimizer:** Winner is selected based on the highest metric value (open rate, click rate, or lowest unsubscribe rate). No explicit statistical significance calculation is documented.
- **Einstein Content Testing:** Uses a multi-armed bandit algorithm that continuously optimizes based on engagement data. Statistical significance is handled internally by the algorithm.
- **Recommended Sample Sizes:** 5% per condition for audiences over 50,000; 10% per condition for smaller audiences. Tests with fewer than 500 subscribers may not be statistically meaningful.
- **Winner Criteria:** Unique open rate, unique click-through rate, or (for Path Optimizer) lowest unsubscribe rate.
- **Tie-Breaking:** In Email Studio, if results are tied, Condition A is declared the winner by default.
- **Test Duration:** 24-48 hours recommended for Email Studio. Path Optimizer duration must account for any wait periods within the paths.

#### Notable Strengths
- Path Optimizer supports up to 10 variants with multi-channel testing (email, SMS, push)
- Einstein Content Testing provides real-time, AI-powered optimization
- Path Optimizer integrates directly into Journey Builder for seamless journey-level testing
- Strong ecosystem with Salesforce CRM integration
- Manual winner selection option allows considering external data

#### Notable Limitations
- Email Studio A/B testing is limited to 2 variants and single-send emails only (not automations or journeys)
- No native statistical significance calculation in Email Studio
- Path Optimizer holdback groups only work for "Run Once" journeys
- Split functionality between Email Studio and Journey Builder creates a fragmented experience
- Einstein features consume "super messages" (additional cost)
- Apple Mail Privacy Protection can inflate open rate data and skew results

#### Source Links
- [A/B Test Email Sends (Email Studio)](https://help.salesforce.com/s/articleView?id=sf.mc_co_ab_test.htm&language=en_US&type=5)
- [A/B Testing in Email Studio](https://help.salesforce.com/s/articleView?id=mktg.mc_es_ab_testing.htm&language=en_US&type=5)
- [Path Optimizer Test Activity in Journey Builder](https://help.salesforce.com/s/articleView?id=mktg.mc_jb_path_optimizer.htm&language=en_US&type=5)
- [Einstein Content Testing](https://help.salesforce.com/s/articleView?id=sf.mc_pb_einstein_content_testing.htm&language=en_US&type=5)
- [Guide to Path Optimizer (Salesforce Ben)](https://www.salesforceben.com/guide-to-path-optimizer-in-marketing-cloud-journey-builder/)
- [Step-by-Step SFMC A/B Testing Guide (Email Uplers)](https://email.uplers.com/blog/step-by-step-guide-to-sfmc-ab-testing/)
- [Trailhead: Path Optimizer in Journey Builder](https://trailhead.salesforce.com/content/learn/modules/path-optimizer-in-journey-builder/create-a-path-optimizer-activity)

---

### 3. Adobe Journey Optimizer (AJO)

**Overview:** Adobe Journey Optimizer is an enterprise-grade customer journey orchestration platform built on the Adobe Experience Platform. It supports omnichannel journey management across email, push, SMS, in-app, web, and code-based experiences. It positions itself as a leader in data-driven personalization and real-time customer engagement.

#### A/B Testing Implementation

**Test Types and Scope:**
- **Content Experiments (A/B Test):** Fixed traffic split between treatments. Marketers define the audience allocation at setup, and performance is measured against a chosen primary metric.
- **Multi-Armed Bandit:** Automatically adjusts traffic distribution every 7 days, directing more traffic to better-performing treatments. Available for inbound channels, unitary journeys, API-triggered campaigns, and recurring outbound channels.
- **Bring Your Own Multi-Armed Bandit:** Allows custom traffic adjustment logic through the Experiment APIs.
- **Experimentation Accelerator (separate license):** An AI-first application that uses agentic AI to analyze experiment results, suggest new treatments, and help teams iterate faster. Launched in 2025/2026.

**Supported Channels:**
- Inbound channels (web, in-app, code-based experience): Available in any journey or campaign.
- Outbound channels (email, push, SMS): Supported in API-triggered transactional campaigns.
- Direct mail: Supports holdout groups but not treatments.

**Important Limitation:** Content experiments are primarily a campaigns feature. Full A/B testing within journeys is limited. Multiple community posts confirm this constraint.

**Technical Approach:**
- Server-side execution with deterministic assignment.
- Uses MurmurHash3 32-bit algorithm to hash user identity into 10,000 buckets, ensuring consistent treatment assignment across multiple exposures.
- Audience allocation is configured per treatment (e.g., 45% Treatment A, 45% Treatment B, 10% holdout).
- When there are more than two treatments, Bonferroni correction is applied for multiple comparisons.

**Holdout Groups:** Supported with a configurable percentage (default 10%). Holdout profiles receive no content but continue through subsequent journey actions.

**Auto-Scaling Winners:**
- Can be configured to scale automatically when a winner is identified, after a specific duration, or with fallback behavior if no winner emerges.
- Manual scaling is also available through the Content Experiment dashboard (processing takes up to one hour).

**API Access:** Adobe provides Experiment APIs for the "Bring Your Own Multi-Armed Bandit" approach, giving programmatic control over traffic adjustment. This is the most advanced API-level experiment control among the platforms analyzed.

#### User Experience and Workflow

**Setup Process:**
1. Create and configure a campaign (or journey action).
2. Click "Create experiment" from the Actions tab.
3. Select the experiment type (A/B, Multi-Armed Bandit, or Bring Your Own MAB).
4. Choose a success metric (Email Opens, Clicks, Page Views, etc.).
5. Add treatments and customize content for each.
6. Assign audience percentages per treatment or use "Distribute evenly."
7. Configure holdout group (optional).
8. Set up auto-scaling rules or plan for manual winner selection.
9. Launch the experiment.

**Complexity Level:** High. Adobe Journey Optimizer is an enterprise platform that requires significant technical expertise. The content experiment feature itself involves understanding statistical concepts, identity namespaces for randomization, and proper metric configuration. The Experimentation Accelerator (separate license) aims to simplify this with AI assistance.

**Visual Configuration:** Treatments are configured within the campaign editor. The experiment dashboard provides visualization of treatment performance over time.

**Results Presentation:** Results are shown in a Content Experiment dashboard with treatment-level performance metrics. The platform indicates when results are "Conclusive" (confidence exceeds 95%). An "Experimentation Report" provides detailed statistical analysis.

#### Statistical Methodology and Analytics

- **Method:** "Anytime valid" confidence sequences (a modern statistical approach). This is the most sophisticated methodology among the platforms analyzed.
- **Key Advantage:** Unlike traditional fixed-horizon methods, anytime valid confidence sequences allow safe interpretation of results at any point during the experiment without increasing false positive risk. Marketers can check results whenever they want without compromising statistical validity.
- **Significance Threshold:** 95% confidence. The experiment is declared "Conclusive" when anytime valid confidence exceeds 95% for at least one treatment.
- **Multiple Comparisons:** Bonferroni correction is applied when comparing more than two treatments, controlling the family-wise error rate.
- **Metrics Types:**
  - Direct metrics: User directly responds (email opens, clicks).
  - Indirect/bottom-of-funnel metrics: Purchases or conversions happening post-exposure (within a 7-day attribution window).
- **Confidence Intervals:** Wider than traditional fixed-horizon confidence intervals (a tradeoff for the ability to safely peek at results anytime).
- **Technical Foundation:** Based on martingale theory. Uses inverse propensity weighted (IPW) estimators.
- **Minimum Test Duration:** At least 7 days recommended for determining a winning treatment.

#### Notable Strengths
- Most advanced statistical methodology (anytime valid confidence sequences) allowing safe early peeking
- Multi-armed bandit with automatic traffic reallocation every 7 days
- "Bring Your Own MAB" with Experiment APIs for programmatic control
- Bonferroni correction for multiple comparisons
- Experimentation Accelerator with agentic AI (future-looking)
- Supports both direct and indirect (bottom-of-funnel) metrics with 7-day attribution

#### Notable Limitations
- Content experiments primarily work in campaigns, not fully in journeys (a significant gap)
- Outbound channel experiments (email, push, SMS) require API-triggered campaigns
- High complexity; requires technical expertise to set up and interpret
- Distribution issues reported with small audiences (all participants may receive the same treatment)
- Experimentation Accelerator requires a separate license
- 7-day minimum test duration may be too long for some use cases
- Auto-selection of winners for holdout targeting is on the roadmap but not yet fully available

#### Source Links
- [Get Started with Content Experiment](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/content-experiment/get-started-experiment)
- [Create a Content Experiment](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/content-experiment/content-experiment)
- [Statistical Calculations in Experimentation](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/content-experiment/technotes/experiment-calculations)
- [A/B vs Multi-Armed Bandit Experiments](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/content-experiment/technotes/mab-vs-ab)
- [Experimentation Accelerator](https://business.adobe.com/products/journey-optimizer/experimentation-accelerator.html)
- [Use Experimentation in Message Optimization](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/message-optimization/optimization-experimentation)

---

### 4. Iterable

**Overview:** Iterable is a cross-channel marketing platform that serves growth-stage and mid-market companies. It supports email, SMS, push notifications, in-app messages, and web push. Iterable positions itself as a marketer-friendly platform with strong workflow automation and experimentation capabilities.

#### A/B Testing Implementation

**Test Types and Scope:**
- **Campaign-level experiments:** A/B tests on individual campaigns (blast, triggered, or journey campaigns). Tests compare variant content against a control.
- **Journey-level testing:** A/B Split tiles within journeys allow testing of entire journey branches (different sequences, channels, or timing).
- **Cross-channel experiments:** Can compare performance of the same content across different channels (e.g., email vs. SMS).
- **Send-Time Experiments:** Test different send times and days across email, SMS, and push.
- **Holdout groups:** A portion of the audience receives no campaign, allowing measurement of baseline conversion rates.

**Supported Channels:** Email, SMS, push notifications, in-app messages, and web push.

**Technical Approach:**
- Server-side execution.
- For blast campaigns: A configurable percentage of the list participates in the experiment, and the winner is sent to the rest.
- For triggered/journey campaigns: Users are randomly assigned to variants as they enter the campaign. Once each variant reaches the minimum send threshold, Iterable uses a multi-armed bandit algorithm to pick a winner.
- The multi-armed bandit sends the winning variant 90% of the time and continues testing other variants at 10%, allowing ongoing optimization and winner re-evaluation.

**Winner Selection:**
- **Automatic:** Iterable determines the winner after the test duration (blast) or minimum sends per variant (triggered). For triggered campaigns, it uses a multi-armed bandit that continues to monitor and can change the winner if user preferences shift.
- **Manual:** Users can click "Declare winner" at any time to end the experiment and promote a specific variant.

**API Access:** Iterable provides the `/experiments/metrics` API endpoint for programmatic access to experiment analytics data. When tracking conversions via API, the `templateId` field must be provided alongside `campaignId` to attribute events to the correct variant. The API supports exporting experiment performance data in CSV format.

#### User Experience and Workflow

**Setup Process:**
1. Create a campaign (or navigate to a journey tile).
2. Under "Conversions and Experiments," click "Add experiment."
3. Name the experiment and select the experiment type.
4. Create campaign variants (clone and modify content).
5. Configure the test strategy:
   - "Test with all users" (100% split, equal distribution)
   - "Test with a subset of users and optimize" (test portion, then send winner to rest)
6. Choose the winning metric (opens, clicks, custom conversion events, or purchases).
7. For blast campaigns: Set the percentage of the list and time window for testing.
8. For triggered campaigns: Set the minimum sends per variant.
9. Launch the experiment.

**Complexity Level:** Moderate. Iterable provides a drag-and-drop experiment builder, but the overall platform has a learning curve. Users report needing "too many clicks to achieve anything" and a cluttered interface when handling multiple campaigns.

**Results Presentation:** The experiment analytics view shows side-by-side metrics for each variant, including sends, delivery rate, click rate, revenue, unique conversion rate, lift, and confidence intervals. Results can be exported to CSV or pulled via API.

#### Statistical Methodology and Analytics

- **Method:** Multi-armed bandit algorithm for triggered/journey campaigns. For blast campaigns with a subset test, traditional comparison with winner declared after the test window.
- **Confidence Metric:** Displays "confidence" as the likelihood that a variant will increase or decrease the conversion rate compared to the control.
- **Confidence Intervals:** Optional display showing the range of expected lift values (e.g., "5pp +/- 2" means you can expect 3pp to 7pp lift).
- **Winner Metric Options:** Opens, clicks, custom conversion events, or purchases. Note: SMS clicks cannot be used to determine the winner.
- **Continuous Monitoring:** For triggered campaigns, Iterable continues monitoring after declaring a winner and will change the winner if user preferences shift.
- **Specific Significance Threshold:** Not explicitly documented in public materials. The platform shows confidence percentages but the exact threshold for "statistically significant" is not publicly specified.

#### Notable Strengths
- Multi-armed bandit with continuous re-evaluation of winners for triggered/journey campaigns
- Cross-channel experiment support (can compare email vs. SMS performance)
- Send-Time Experiments across email, SMS, and push
- A/B Split tiles in journeys for testing entire journey branches
- API access for experiment metrics (`/experiments/metrics` endpoint)
- Holdout groups for measuring incremental campaign impact

#### Notable Limitations
- SMS clicks cannot be used as a winning metric for experiments
- No explicit documentation of statistical significance thresholds or methodology details
- User interface described as cluttered with too many clicks required
- Reporting capabilities described as a weak point by reviewers
- Adding experiments to already-active campaigns reduces data quality (can't retroactively evaluate earlier sends)
- Platform has a steep learning curve, especially for automation first-timers

#### Source Links
- [Experiments Overview](https://support.iterable.com/hc/en-us/articles/205480325-Experiments-Overview)
- [Configuring Experiments](https://support.iterable.com/hc/en-us/articles/22872099556116-Configuring-Experiments)
- [Creating an Experiment](https://support.iterable.com/hc/en-us/articles/11270793431956-Creating-an-Experiment)
- [Experiment Analytics / A/B Experiment Metrics](https://support.iterable.com/hc/en-us/articles/115000823186-A-B-Experiment-Metrics)
- [Planning an Experiment](https://support.iterable.com/hc/en-us/articles/11270739567380-Planning-an-Experiment)
- [Launching an Experiment](https://support.iterable.com/hc/en-us/articles/22838401993236-Launching-an-Experiment)
- [Send-Time Experiments Blog](https://iterable.com/blog/send-time-experiments/)

---

### 5. Klaviyo

**Overview:** Klaviyo is a marketing automation platform with particular strength in e-commerce and the SMB/mid-market segment. It focuses on email, SMS, and push notifications with deep integrations into e-commerce platforms like Shopify. Klaviyo is known for its accessible interface and data-driven approach.

#### A/B Testing Implementation

**Test Types and Scope:**
- **Campaign email A/B testing:** Test subject lines, email content, or send times.
- **Campaign SMS A/B testing:** Test SMS message content (MMS vs. SMS, emojis, CTAs).
- **Flow email A/B testing:** Test email variants within automated flows (welcome series, abandoned cart, etc.).
- **Flow SMS A/B testing:** Test SMS message content within flows using split logic.
- **Sign-up form A/B testing:** Test different sign-up form designs and content.

**Supported Channels:** Email and SMS (for A/B testing). Push notification support exists on the platform but A/B testing for push is not prominently documented.

**Technical Approach:**
- Server-side execution.
- For campaigns: A test portion of the audience receives variants, then the winning variant is sent to the remainder.
- For flows: As users enter the flow, they are randomly assigned to variants. Once at least 500 recipients have received each variation and a 90% win probability is achieved, a winner is declared.
- Klaviyo recommends a minimum of 100 recipients per variation for campaign tests and 500 per variation for flow tests.

**Personalized Campaigns (AI-powered):**
- Available for accounts with more than 400,000 total profiles.
- Uses customer behavior data (engagement rates, lifetime value, location) to determine which variant each individual recipient should receive.
- Instead of picking one global winner, each subscriber gets the message variant best suited to them.

**Control/Holdback Groups:** Klaviyo's A/B testing does not include a dedicated control/holdback group feature. Tests split traffic between active variants, but there is no built-in option to withhold messages from a portion of the audience to measure incremental impact.

**Number of Variants:** Klaviyo recommends only 2 variations per test, though additional variants can be created by cloning.

**API Access:** Klaviyo's Campaigns API supports creating, sending, and managing campaigns, but does not appear to have dedicated endpoints for creating or managing A/B tests programmatically. A/B test setup and management is done through the Klaviyo UI.

#### User Experience and Workflow

**Campaign A/B Test Setup:**
1. Navigate to Campaigns > Create Campaign.
2. Select Email or SMS and configure recipients.
3. Draft the initial message content.
4. Click "Create A/B test" (which auto-generates a second identical variation).
5. Modify the second variation (change subject line, content, or send time).
6. Name each variation descriptively.
7. Choose the winning metric (Open Rate, Click Rate, or Placed Order Rate).
8. Set the test size (percentage of audience) and test duration.
9. Select the test strategy (Winning Variation or Personalized Campaigns, if eligible).
10. Review and launch.

**Flow A/B Test Setup:**
1. Navigate to the flow where you want to test.
2. Drag an email or SMS action into the flow.
3. Configure the content.
4. Click "Create A/B test" to create a copy.
5. Modify the second variation.
6. Set variant weights (percentage distribution).
7. Choose the winning metric (Click Rate or Open Rate).
8. Configure automatic winner selection or manual end date.
9. Activate.

**Complexity Level:** Low to moderate. Klaviyo is generally considered one of the more accessible platforms for A/B testing. The one-click "Create A/B test" button streamlines the process. However, the template builder has reported bugs, and advanced features like Personalized Campaigns are gated behind a high profile count threshold.

**Results Presentation:** Results show win probability, statistical significance status, and comparative metrics. Results are categorized as "Statistically Significant," "Promising," or "Inconclusive." Real-time tracking of open rates, click rates, and conversion rates is available on the dashboard.

#### Statistical Methodology and Analytics

- **Method:** Win probability model based on the selected metric.
- **Significance Threshold (Flows):** A result is considered statistically significant when (a) at least 500 recipients have received each variation and (b) a variation has at least 90% win probability.
- **Significance Threshold (Campaigns):** Similar win probability approach. Results are classified as "Statistically Significant" (high win probability, 90%+), "Promising" (one variant looks better but evidence isn't strong enough), or "Inconclusive" (not enough data).
- **Winning Metrics:** Open Rate (best for subject line tests), Click Rate (best for content tests), Placed Order Rate (for accounts with conversion tracking; not available with Personalized Campaigns strategy).
- **Automatic Winner Selection:** Default for flows. The test ends when statistical significance is reached or a specified date is reached, whichever comes first.
- **Early Stopping:** Supported. Users can manually choose a winner at any time. Changing a flow message from "live" to "draft" status auto-selects a winner (first variation wins in case of a tie).

#### Notable Strengths
- Very accessible setup process with one-click A/B test creation
- A/B testing available in both campaigns AND flows (automated sequences)
- AI-powered Personalized Campaigns for per-recipient variant selection (for large accounts)
- Sign-up form A/B testing (unique among competitors analyzed)
- Clear result classification (Significant, Promising, Inconclusive)
- SMS A/B testing for both campaigns and flows

#### Notable Limitations
- Recommends only 2 variations (limited multivariate testing capability)
- Personalized Campaigns requires 400,000+ profiles (excludes most smaller customers)
- No native statistical significance calculation beyond win probability
- Cannot modify a live A/B test; must cancel and restart
- Placed Order Rate not available with Personalized Campaigns strategy
- No dedicated API for A/B test management
- Template builder has reported bugs
- Apple Mail Privacy Protection can inflate open rate data

#### Source Links
- [How to A/B Test an Email Campaign](https://help.klaviyo.com/hc/en-us/articles/115005228148)
- [How to A/B Test SMS Campaigns](https://help.klaviyo.com/hc/en-us/articles/4406796460443)
- [How to A/B Test Flow Emails](https://help.klaviyo.com/hc/en-us/articles/6960371049115)
- [How to A/B Test Flow SMS Messages](https://help.klaviyo.com/hc/en-us/articles/360036238671)
- [Understanding Statistical Significance in Flows](https://help.klaviyo.com/hc/en-us/articles/11233978755611)
- [Understanding Statistical Significance in Campaigns](https://help.klaviyo.com/hc/en-us/articles/360052793012)
- [How to Review A/B Test Results for Campaigns](https://help.klaviyo.com/hc/en-us/articles/360052794352)
- [Best Practices for A/B Testing](https://help.klaviyo.com/hc/en-us/articles/360045012632)
- [Email A/B Testing Product Page](https://www.klaviyo.com/products/email-marketing/ab-testing)
- [Campaigns API Overview](https://developers.klaviyo.com/en/reference/campaigns_api_overview)

---

## Cross-Platform Patterns

After analyzing all five competitors, several common approaches emerge:

### Standard Features Present in Most/All Platforms

| Feature | Braze | SFMC | Adobe JO | Iterable | Klaviyo |
|---|:---:|:---:|:---:|:---:|:---:|
| Email A/B testing | Yes | Yes | Yes | Yes | Yes |
| SMS A/B testing | Yes | Yes (Path Opt.) | Yes (API-triggered) | Yes | Yes |
| Push A/B testing | Yes | Yes (Path Opt.) | Yes (API-triggered) | Yes | Limited |
| Journey/path testing | Yes | Yes | Limited | Yes | Via flow splits |
| Control/holdback groups | Yes | Partial | Yes | Yes | No (not documented) |
| Automatic winner selection | Yes | Yes | Yes | Yes | Yes |
| Multiple variants (3+) | Yes (up to 8 campaign / 4 journey) | Yes (up to 10) | Yes (limit not documented) | Yes (limit not documented) | Limited (2 recommended) |
| Real-time results | Yes | Yes | Yes | Yes | Yes |

### Common Technical Patterns

1. **Server-side variant assignment:** All platforms assign variants on the server side when the message is sent or the user enters the journey. None rely on client-side assignment for messaging campaigns.

2. **Random distribution with configurable splits:** All platforms allow marketers to define the percentage of audience allocated to each variant. Most default to even splits.

3. **Two-phase approach for campaign tests:** Most platforms use a "test then send winner" model for blast campaigns: test with a subset of the audience, determine the winner, then send the winning variant to the remaining audience.

4. **Persistent variant assignment:** When users can re-enter a campaign or journey, they receive the same variant they were originally assigned (Braze, Adobe).

### Typical User Workflows

1. **Campaign creation first, then experiment setup:** All platforms require creating the base campaign before adding A/B test configuration. The experiment is layered on top of the campaign, not the other way around.

2. **3-7 step setup process:** Most platforms require 5-7 steps to set up an A/B test, from creating the campaign to launching the experiment. Klaviyo has the simplest flow (one-click test creation), while Adobe has the most complex.

3. **Visual side-by-side variant comparison:** All platforms show variant content and results side by side for easy comparison.

4. **Results categorization:** Most platforms provide some form of statistical confidence indicator, whether as a percentage (Braze, Iterable), category labels (Klaviyo), or conclusiveness determination (Adobe).

### Standard Statistical Approaches

1. **95% confidence is the most common threshold.** Braze and Adobe both use 95%. Klaviyo uses 90% win probability. Salesforce does not calculate significance natively.

2. **Winner determination defaults to highest metric value.** If statistical significance is not reached, most platforms still promote the best-performing variant (Braze, Salesforce, Klaviyo).

3. **Open rate and click rate are the universal winning metrics.** All platforms support these. Some add conversion rate (Klaviyo: Placed Order Rate; Iterable: custom conversion events; Braze: primary conversion events).

---

## Differentiated Capabilities

These features stand out as unique or advanced capabilities that set certain platforms apart:

### 1. Anytime Valid Confidence Sequences (Adobe Journey Optimizer)

Adobe's use of "anytime valid" statistical methods is the most sophisticated approach among the platforms analyzed. Unlike traditional methods that require waiting until a fixed sample size is reached, this approach allows marketers to check results at any time without increasing the risk of false positives. This is a meaningful advantage because a common problem with A/B testing is "peeking" at results too early and making premature decisions.

**Why it matters for Infobip:** This approach addresses a real pain point. Marketers frequently check test results before the test period is over, which can lead to wrong conclusions with traditional statistics. Implementing a similar approach would be a genuine differentiator.

### 2. Personalized Variants/Paths (Braze)

Braze's approach goes beyond picking a single winning variant. It uses machine learning (logistic regression) to identify which user characteristics predict which variant a user will prefer, then sends each user their individually optimal variant. If no meaningful patterns are found, it falls back to the overall winning variant.

**Why it matters for Infobip:** This turns A/B testing from "find one winner for everyone" into "find the best message for each person." It's a step toward 1:1 personalization built on top of experimentation.

### 3. Multi-Armed Bandit with Continuous Re-evaluation (Iterable)

For triggered/journey campaigns, Iterable's multi-armed bandit sends the winning variant 90% of the time while continuing to test other variants at 10%. If user preferences change, the system automatically switches to a new winning variant.

**Why it matters for Infobip:** This is especially valuable for long-running triggered campaigns where audience behavior may shift over time (e.g., seasonal changes, market shifts).

### 4. Path Optimizer with 10 Paths (Salesforce Marketing Cloud)

Salesforce's Path Optimizer allows testing up to 10 different journey paths simultaneously, each potentially containing different channels, timing, and content. This goes beyond message-level A/B testing into true journey-level experimentation.

**Why it matters for Infobip:** Testing 10 paths simultaneously enables more ambitious experiments, such as comparing entirely different customer journeys rather than just message variants.

### 5. Sign-Up Form A/B Testing (Klaviyo)

Klaviyo uniquely offers A/B testing for sign-up forms (pop-ups, embedded forms), not just messages. This extends experimentation to the audience acquisition stage.

**Why it matters for Infobip:** While Infobip Moments may not have sign-up forms, the principle of extending A/B testing beyond messaging into other customer touchpoints is worth considering.

### 6. Feature Flag A/B Testing (Braze)

Braze connects A/B testing to feature flags, allowing product and engineering teams to test new in-app or on-site features alongside marketing messages.

**Why it matters for Infobip:** This bridges the gap between marketing experimentation and product experimentation, enabling a more holistic approach to customer experience optimization.

### 7. Experimentation Accelerator with Agentic AI (Adobe)

Adobe's Experimentation Accelerator (launched 2025/2026, separate license) uses AI to automatically analyze experiment results, explain why certain treatments outperformed others, and suggest new experiments.

**Why it matters for Infobip:** AI-assisted experiment analysis could significantly lower the barrier for non-technical marketers to run and learn from experiments.

---

## Gaps and Opportunities

### Features Commonly Requested but Not Widely Available

1. **Cross-channel A/B testing within a single experiment.** While Iterable allows comparing email vs. SMS, and Salesforce Path Optimizer can compare paths using different channels, a truly unified cross-channel experiment is not standard. A single experiment that tests the optimal channel (or channel combination) for each user segment would be highly valuable.

2. **Full journey-level experimentation (not just path splits).** Adobe Journey Optimizer notably lacks full content experiment support within journeys. Even on platforms that support it, journey-level experiments are often limited to path splits rather than intelligent optimization across the entire journey.

3. **Native statistical significance in all test contexts.** Salesforce does not calculate statistical significance natively in Email Studio. Several platforms leave statistical interpretation to the marketer. There is an opportunity to provide clear, built-in guidance.

4. **A/B testing APIs for programmatic experiment management.** None of the platforms offer comprehensive APIs for creating, managing, and analyzing A/B tests programmatically. This is a gap for teams that want to integrate experimentation into their development workflows or build custom reporting.

5. **Real-time experiment adjustments.** Most platforms do not allow modifying a live experiment. Braze disables the experiment entirely if changes are made. Only multi-armed bandit implementations (Adobe, Iterable) adapt during the test, and even these have constraints.

### Limitations Mentioned Across Platforms

1. **Cannot modify live experiments.** Braze, Klaviyo, and Adobe all prevent changes once an experiment is running. This is statistically sound but frustrating for marketers who spot errors.

2. **Apple Mail Privacy Protection inflates open rates.** Multiple platforms (Salesforce, Klaviyo) acknowledge that Apple's privacy features make open-rate-based testing unreliable. This is an industry-wide problem that no platform has fully solved.

3. **Small audience distribution issues.** Both Adobe and Salesforce note problems with statistical validity at small audience sizes. Adobe specifically has reported issues where all participants in small tests receive the same treatment.

4. **Steep learning curves for advanced features.** Braze and Adobe are both criticized for complexity. Iterable's interface is described as cluttered. There is a consistent tension between power and usability.

5. **Limited reporting depth.** Both Iterable and Salesforce receive criticism for reporting limitations. Braze caps historical data storage. None of the platforms offer a centralized experiment knowledge base or learning repository (though Adobe's Experimentation Accelerator moves in this direction).

### Potential Areas for Differentiation

1. **Unified experiment dashboard** showing all active and completed experiments across campaigns and journeys, with searchable insights and learnings.

2. **Guided experiment setup** that recommends test types, sample sizes, and durations based on the audience size and historical data, reducing the need for statistical expertise.

3. **Cross-channel optimization experiments** that test not just which message content works best, but which channel (or channel combination) works best for each user segment.

4. **Experiment templates** (pre-built experiment designs for common scenarios like welcome series optimization, re-engagement testing, or send-time optimization).

5. **Comprehensive experimentation API** that enables programmatic creation, management, and analysis of experiments.

---

## Strategic Recommendations

Based on this research, here are actionable recommendations for Infobip Moments:

### 1. Core Capabilities to Implement (Table Stakes)

These features are present in most or all competitors and are expected by the market:

- **Campaign-level A/B testing** for email, SMS, and push with at least 2 variants (ideally up to 8).
- **Control/holdback groups** to measure the incremental effect of messaging vs. no messaging.
- **Automatic winner selection** based on configurable metrics (open rate, click rate, conversion rate).
- **Configurable audience allocation** with percentage-based splits across variants.
- **Two-phase testing** for blast campaigns: test with a subset, then send the winner to the rest.
- **Statistical significance indicators** showing whether results are reliable (95% confidence is the industry standard).
- **Visual side-by-side variant comparison** for both configuration and results.
- **Real-time results tracking** during the test period.

### 2. Differentiating Features to Consider (Competitive Advantages)

These features would position Infobip Moments ahead of most competitors:

- **Journey-level path testing** (similar to Braze Experiment Paths or Salesforce Path Optimizer) allowing testing of entire journey branches including different channels, timing, and message sequences. Aim for at least 4-5 testable paths (this balances experiment complexity with actionability, covering the most common use cases while exceeding Braze's 4-path limit).
- **Multi-armed bandit option** for triggered/journey campaigns that automatically shifts traffic to better-performing variants while continuing to explore alternatives. This is available in Adobe and Iterable but not universally.
- **Personalized variant selection** using ML to send each user the variant most likely to work for them. Braze and Klaviyo offer this, but it's still rare enough to be a differentiator.
- **Cross-channel A/B testing** that compares not just message content but which channel (email vs. SMS vs. push) works best for each segment. Iterable supports this; most others do not.
- **Anytime valid statistics** (like Adobe's approach) that allow safe peeking at results without statistical penalty. This would be a strong differentiator as most competitors use simpler fixed-horizon methods.

### 3. Implementation Approaches to Evaluate (Technical Strategy)

- **Server-side variant assignment** is the clear standard. All competitors use this approach for messaging experiments. It provides reliable, deterministic assignment.
- **Deterministic hashing for assignment** (as Adobe does with MurmurHash3) ensures consistent treatment when users encounter the experiment multiple times.
- **Start with a two-phase model** (test then send winner) for blast campaigns, and a continuous multi-armed bandit for triggered/journey campaigns.
- **Build experiment APIs from the start.** No competitor offers a comprehensive experimentation API. This is a greenfield opportunity to attract technically sophisticated teams. An API should cover experiment creation, management, variant configuration, and results retrieval.
- **Separate experiment configuration from campaign configuration.** Make the experiment a first-class entity that can be attached to campaigns, flows, or journey paths, rather than deeply embedded in the campaign setup. This enables more flexible experimentation and better analytics.

### 4. User Experience Principles to Adopt (Workflow and Design)

- **One-click test creation** (like Klaviyo's "Create A/B test" button) that auto-generates a duplicate variant. This lowers the barrier to starting experiments.
- **Guided setup with recommendations.** Based on the audience size and historical data, recommend the optimal test size, duration, and winning metric. Address the common problem of tests with too-small audiences.
- **Clear result categories.** Use plain-language labels like Klaviyo's "Statistically Significant," "Promising," and "Inconclusive" instead of raw numbers. Show confidence percentages for technical users who want them, but lead with accessible labels.
- **Centralized experiment dashboard.** Provide a single view of all experiments across all campaigns and journeys with search, filter, and trend analysis. No competitor does this well.
- **Experiment templates.** Offer pre-built experiment designs for common scenarios (subject line test, send time test, channel comparison, journey path test) to help marketers get started quickly.
- **Learning repository.** Store the results and insights from all experiments in a searchable knowledge base so teams can build on past learnings. Adobe's Experimentation Accelerator is the only competitor moving in this direction.
- **Accessible language in the UI.** Avoid statistical jargon in the interface. Instead of "p-value < 0.05," show "We're 95% confident this result is real, not random chance."
- **Handle Apple Mail Privacy Protection.** Open rates are increasingly unreliable due to Apple MPP pre-fetching emails. Default to click-based winning metrics, or offer a toggle to exclude suspected machine opens. Multiple competitors (Salesforce, Klaviyo) acknowledge this problem but none have fully solved it.

---

## Sources

### Braze
- [Multivariate and A/B Testing Overview](https://www.braze.com/docs/user_guide/engagement_tools/testing/multivariant_testing)
- [Create Multivariate and A/B Tests](https://www.braze.com/docs/user_guide/engagement_tools/testing/multivariant_testing/create_multivariate_campaign)
- [Multivariate and A/B Test Analytics](https://www.braze.com/docs/user_guide/engagement_tools/testing/multivariant_testing/multivariate_analytics)
- [Optimizations: Winning Variant & Personalized Variants](https://www.braze.com/docs/user_guide/engagement_tools/testing/multivariant_testing/optimizations)
- [Canvas Experiment Paths](https://www.braze.com/docs/user_guide/engagement_tools/canvas/canvas_components/experiment_step)
- [Personalized Paths in Canvas](https://www.braze.com/docs/user_guide/engagement_tools/canvas/canvas_components/experiment_step/personalized_paths)
- [Winning Path in Experiment Paths](https://www.braze.com/docs/user_guide/engagement_tools/canvas/canvas_components/experiment_step/winning_path)
- [Feature Flag A/B Testing](https://www.braze.com/resources/articles/building-braze-feature-flags-ab-testing)
- [AI A/B Testing Guide](https://www.braze.com/resources/articles/ai-ab-testing)
- [Braze API Guide](https://www.braze.com/docs/api/home)
- [Braze Reviews on Capterra](https://www.capterra.com/p/155423/Appboy/reviews/)
- [Braze Reviews on TrustRadius](https://www.trustradius.com/products/braze/reviews/all)

### Salesforce Marketing Cloud
- [A/B Test Email Sends](https://help.salesforce.com/s/articleView?id=sf.mc_co_ab_test.htm&language=en_US&type=5)
- [A/B Testing in Email Studio](https://help.salesforce.com/s/articleView?id=mktg.mc_es_ab_testing.htm&language=en_US&type=5)
- [Path Optimizer Test Activity in Journey Builder](https://help.salesforce.com/s/articleView?id=mktg.mc_jb_path_optimizer.htm&language=en_US&type=5)
- [Configure a Path Optimizer Test Activity](https://help.salesforce.com/s/articleView?id=mktg.mc_jb_path_optimizer_test_activity.htm&language=en_US&type=5)
- [A/B Test Summary Report](https://help.salesforce.com/s/articleView?id=sf.mc_re_ab_test_summary_report.htm&language=en_US&type=5)
- [Einstein Content Testing](https://help.salesforce.com/s/articleView?id=sf.mc_pb_einstein_content_testing.htm&language=en_US&type=5)
- [Trailhead: Path Optimizer in Journey Builder](https://trailhead.salesforce.com/content/learn/modules/path-optimizer-in-journey-builder/create-a-path-optimizer-activity)
- [Trailhead: Test and Launch a Journey Campaign](https://trailhead.salesforce.com/content/learn/modules/journey-builder-campaigns/test-and-launch-a-journey-campaign)
- [Guide to Path Optimizer (Salesforce Ben)](https://www.salesforceben.com/guide-to-path-optimizer-in-marketing-cloud-journey-builder/)
- [Step-by-Step SFMC A/B Testing (Email Uplers)](https://email.uplers.com/blog/step-by-step-guide-to-sfmc-ab-testing/)
- [Path Optimizer Deep Dive (Consulting the Cloud)](https://consultingintheclouds.com/2020/08/03/ab-n-testing-in-journey-builder-now-by-simple-drag-and-drop/)

### Adobe Journey Optimizer
- [Get Started with Content Experiment](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/content-experiment/get-started-experiment)
- [Create a Content Experiment](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/content-experiment/content-experiment)
- [Statistical Calculations in Experimentation](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/content-experiment/technotes/experiment-calculations)
- [Experimentation Report Calculations](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/content-experiment/technotes/experiment-report-calculations)
- [A/B vs Multi-Armed Bandit Experiments](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/content-experiment/technotes/mab-vs-ab)
- [Use Experimentation in Message Optimization](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/message-optimization/optimization-experimentation)
- [Adobe Experimentation Accelerator Product Page](https://business.adobe.com/products/journey-optimizer/experimentation-accelerator.html)
- [Introducing Experimentation Accelerator Blog](https://business.adobe.com/blog/introducing-adobe-journey-optimizer-experimentation-accelerator)
- [Adobe Journey Optimizer Gartner Reviews](https://www.gartner.com/reviews/market/multichannel-marketing-hubs/vendor/adobe/product/adobe-journey-optimizer)

### Iterable
- [Experiments Overview](https://support.iterable.com/hc/en-us/articles/205480325-Experiments-Overview)
- [Configuring Experiments](https://support.iterable.com/hc/en-us/articles/22872099556116-Configuring-Experiments)
- [Creating an Experiment](https://support.iterable.com/hc/en-us/articles/11270793431956-Creating-an-Experiment)
- [Planning an Experiment](https://support.iterable.com/hc/en-us/articles/11270739567380-Planning-an-Experiment)
- [Launching an Experiment](https://support.iterable.com/hc/en-us/articles/22838401993236-Launching-an-Experiment)
- [Managing Experiments](https://support.iterable.com/hc/en-us/articles/11270693278612-Managing-Experiments)
- [A/B Experiment Metrics](https://support.iterable.com/hc/en-us/articles/115000823186-A-B-Experiment-Metrics)
- [Send-Time Experiments Blog](https://iterable.com/blog/send-time-experiments/)
- [Experiments and A/B Testing Academy](https://academy.iterable.com/experiments-ab-testing)
- [What is A/B Testing (Iterable)](https://iterable.com/resources/articles/cross-channel-marketing/general/what-is-a-b-testing/)

### Klaviyo
- [How to A/B Test an Email Campaign](https://help.klaviyo.com/hc/en-us/articles/115005228148)
- [How to A/B Test SMS Campaigns](https://help.klaviyo.com/hc/en-us/articles/4406796460443)
- [How to A/B Test a Flow Email](https://help.klaviyo.com/hc/en-us/articles/6960371049115)
- [How to A/B Test Flow SMS Messages](https://help.klaviyo.com/hc/en-us/articles/360036238671)
- [Understanding Statistical Significance in Flows](https://help.klaviyo.com/hc/en-us/articles/11233978755611)
- [Understanding Statistical Significance in Campaigns](https://help.klaviyo.com/hc/en-us/articles/360052793012)
- [How to Review A/B Test Results for Campaigns](https://help.klaviyo.com/hc/en-us/articles/360052794352)
- [Best Practices for A/B Testing](https://help.klaviyo.com/hc/en-us/articles/360045012632)
- [Understanding the Experiments Section](https://help.klaviyo.com/hc/en-us/articles/18631098703515)
- [Understanding What to A/B Test in Flows](https://help.klaviyo.com/hc/en-us/articles/360054629031)
- [Email A/B Testing Product Page](https://www.klaviyo.com/products/email-marketing/ab-testing)
- [A/B Testing Best Practices Blog](https://www.klaviyo.com/blog/ab-testing-email)
- [Campaigns API Overview](https://developers.klaviyo.com/en/reference/campaigns_api_overview)

### General / Cross-Platform
- [Top 6 Marketing Automation A/B Testing Tools 2025 (Saffron Edge)](https://www.saffronedge.com/blog/ab-testing-tools/)
- [Best A/B Testing Tools for Marketers 2025 (Statsig)](https://www.statsig.com/comparison/best-ab-testing-tools-marketers)
- [Best Marketing Automation Software with A/B Testing (GetApp)](https://www.getapp.com/marketing-software/marketing-automation/f/a-b-testing/)

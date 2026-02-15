<objective>
Build interactive HTML prototypes showing multiple UI approaches for adding A/B testing to the Infobip Moments broadcast campaign creation flow.

Create at least 3 distinct approaches, each as a separate interactive prototype. These prototypes will be used in product discussions to compare tradeoffs and pick a direction.
</objective>

<context>
Read the solution proposal first — it defines what the A/B testing feature does:
@proposals/phase1-ab-testing-broadcast.md

Read the competitive analysis for inspiration on how others handle this:
@research/ab-testing-competitive-analysis.md

<current_broadcast_flow>
The current broadcast creation flow:
1. **Channel selection** — Icon grid to pick one channel (SMS, Email, MMS, Push, RCS, Viber, Voice, WhatsApp, Zalo)
2. **Recipients** — Add audience via People profiles, file upload, or reuse files
3. **Message composition** — Select sender, write content, live device preview, save as template
4. **Settings** — Scheduling, tracking, delivery speed, validity, failover
5. **Preview & Launch** — Name draft, review everything, launch

The UI follows a vertical step-by-step flow. Each step expands/collapses as users progress.
</current_broadcast_flow>

<design_system_context>
Infobip's UI uses:
- Clean, modern enterprise SaaS aesthetic
- Primary color: deep blue/purple tones
- White backgrounds with card-based layouts
- Step indicators or vertical progress flows
- Action buttons: primary (filled, blue/purple), secondary (outlined)
- Form elements: standard inputs, toggles, dropdowns, sliders
- Use a professional sans-serif font (Inter or similar)
</design_system_context>
</context>

<requirements>
Create 3+ interactive HTML prototypes. Each prototype should represent a DIFFERENT UI approach to the same problem: "Where and how does the user set up an A/B test within the broadcast creation flow?"

<approach_1>
**"A/B Toggle" approach — Minimal disruption**
- Add a simple toggle or switch at the top of the message composition step: "Enable A/B Test"
- When enabled, the message editor splits into two tabs (Variant A / Variant B)
- A small config panel appears below for test settings (split %, test duration, winning metric)
- The rest of the flow stays exactly the same
- Tradeoff: Easy to build, minimal learning curve, but A/B testing feels like an add-on
</approach_1>

<approach_2>
**"Dedicated step" approach — A/B as a first-class step**
- Add a new step between Recipients and Message Composition: "Test Setup"
- In this step, the user chooses: "Send to everyone" (no test) or "A/B Test"
- If A/B Test selected, they configure: test mode (equal split vs test-then-send-winner), split %, duration, winning metric
- Then the Message Composition step shows side-by-side editors for Variant A and Variant B
- Tradeoff: Makes A/B testing prominent, but adds a step to every campaign (even non-test ones)
</approach_2>

<approach_3>
**"Branch from creation" approach — Choose campaign type upfront**
- On the initial broadcast creation screen (before channel selection), offer two paths: "Standard Broadcast" and "A/B Test Broadcast"
- Choosing "A/B Test Broadcast" enters a modified flow with variant editors and test configuration built in
- The creation flow adapts based on the choice
- Tradeoff: Clean separation, but duplicates some flow logic; users must decide upfront
</approach_3>

Feel free to add a 4th approach if you see another good pattern from the competitive analysis.

Each prototype must include:
1. **Campaign creation flow** — Show the full flow from start to launch, highlighting where A/B configuration happens
2. **Message composition** — Show how two variants are edited (tabs, side-by-side, stacked, etc.)
3. **Test configuration** — Show the settings panel: split percentage (slider), test duration, winning metric selector, test mode toggle
4. **Preview screen** — Show the launch preview with A/B test summary
5. **Results view** — Show a mock analytics screen comparing Variant A vs Variant B performance with winner indication

Make each prototype interactive:
- Clickable navigation between steps
- Working toggles, tabs, and dropdowns (visual only, no real data)
- Smooth transitions between states
- Responsive enough to demo on a laptop screen
</requirements>

<implementation>
Use a single HTML file per approach with embedded CSS and JavaScript. No external dependencies — everything inline.

Each HTML file should:
- Be self-contained and work by opening in a browser
- Use modern CSS (flexbox, grid, custom properties) for layout
- Use vanilla JavaScript for interactivity (no frameworks)
- Look polished and professional — this will be shown in product meetings
- Include a header bar identifying which approach it demonstrates
- Include brief "About this approach" text explaining the tradeoffs

Design quality matters. These need to look like real product mockups, not wireframes. Use:
- Proper spacing, typography hierarchy, and color
- Realistic placeholder content (actual message examples, not "Lorem ipsum")
- Subtle shadows, borders, and hover states
- Icons where appropriate (use simple SVG or Unicode symbols)

For the results/analytics view, use realistic mock data:
- Variant A: 45.2% open rate, 12.8% CTR
- Variant B: 51.7% open rate, 15.3% CTR
- Show Variant B as the winner with "Confident" status
- Include a simple bar chart visualization (CSS-based, no charting library needed)
</implementation>

<output>
Save each prototype as a separate file:
- `./prototypes/approach-1-toggle.html` — A/B Toggle approach
- `./prototypes/approach-2-dedicated-step.html` — Dedicated Step approach
- `./prototypes/approach-3-branch-creation.html` — Branch from Creation approach
- `./prototypes/approach-4-[name].html` — (if you add a 4th)

Also create an index page:
- `./prototypes/index.html` — Links to all approaches with a brief description of each and its tradeoffs
</output>

<verification>
Before finishing, verify each prototype:
- Opens correctly in a browser as a standalone file
- All navigation/tabs/toggles work
- Shows the complete flow: creation → composition → test config → preview → results
- Looks professional enough for a product meeting
- The "About this approach" text clearly explains tradeoffs
- Mock analytics data is consistent across all prototypes
</verification>

<success_criteria>
- At least 3 distinct, interactive prototypes that show genuinely different UI approaches
- Each prototype tells a clear story: "Here's how a marketer would set up an A/B test with this approach"
- A product manager could open these in a browser and immediately understand the differences
- The index page makes it easy to compare approaches side by side
</success_criteria>

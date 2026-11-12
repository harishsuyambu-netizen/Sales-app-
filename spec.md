# Sales CRM Canvas App Specification

Source: `C:\Users\Hsuyambu\Downloads\Sales CRM.html`

## Design tokens and layout

- **Canvas:** tablet, 1366 × 768. White page, Inter font (fallback: system sans-serif; use the nearest available clean sans-serif if Inter is unavailable).
- **Primary blue:** `#1B5FD9`
- **Light blue:** `#4B8BF5`
- **Navy:** `#0F2A4A`
- **Teal:** `#0EA5A4`
- **Won/converted green:** `#0F9D6B`
- **Text:** `#1E293B`
- **Muted text:** `#64748B`
- **Border:** `#E2E8F0`
- **Panel:** `#F1F5F9`
- **Tint:** `#E6EEFC`
- **Background:** `#FFFFFF`
- **Layout:** 98 px vertical navy sidebar; remaining content area with 26 px horizontal padding. A 5 px blue-to-teal gradient strip spans the top of each screen. At widths below 900 px, navigation becomes a horizontal scrolling row and the two-column dashboard sections stack.
- **Cards:** white fill, 1 px border, 14 px radius, subtle shadow, 18 px padding. Pipeline columns use panel fill and 16 px radius; deal cards use white fill, 14 px radius.
- **Typography:** page titles 26 px bold; body 14 px; muted descriptions 13 px; card titles 15 px semibold; KPI values 28 px bold.
- **Buttons/chips:** pill-shaped, primary actions blue with white text; inactive chips white with border; selected chips blue with white text.
- **Currency:** Indian lakh style, e.g. `₹4.5 L`; values below one lakh use Indian comma grouping.
- All app colors are named formulas in `App.Formulas`; controls must reference those formulas rather than embed color literals.

## Shared navigation and feedback

- Persistent sidebar: white `CRM` logo tile and five icon-plus-label navigation buttons in this order: Dashboard, Leads, Pipeline, Contacts, Activities. Selected item is white with primary-blue text; other items are white on navy.
- Each navigation button opens its corresponding screen. Dashboard is the start screen. Navigation selection follows the visible screen.
- A toast/status notification appears at the bottom center for success feedback (about 2.2 seconds).
- Icons are standalone SVG data URIs with the HTML symbol paths, recolored with concrete token hex values; no PCF or custom code.

## Screen 1 — Dashboard (`scrDashboard`)

- Header: `Sales dashboard`; subtitle `Pipeline, leads and what to do today`.
- Four responsive KPI cards, calculated from the collections:
  1. **Open pipeline:** sum of values for deals before Won; subtext is open-deal count. Highlight card with blue-to-navy gradient.
  2. **Weighted forecast:** sum of deal value × stage win chance for deals before Won; subtext `Value × win chance`.
  3. **Won revenue:** sum of Won deal values; subtext is closed-deal count.
  4. **Lead conversion:** Converted leads ÷ all leads, rounded to whole percent; subtext is count of New leads waiting.
- Two-column row:
  - **Pipeline by stage** card, subtitle `Deals and value`; four rows for Qualified, Proposal, Negotiation, Won. Each row shows stage, count, total value, and a proportional horizontal bar (bar maximum is the largest stage total).
  - **Leads by source** card; source names sorted by descending lead count, count labels, and proportional bars.
- Two-column row:
  - **Today’s activities** card; count of incomplete activities due Today and rows showing type icon, title, company, and time. Empty state: `All clear for today.`
  - **Top open deals** card; up to four non-Won deals sorted by value descending. Each row shows owner initials, deal name, company, stage, and formatted value.

## Screen 2 — Leads (`scrLeads`)

- Header: `Leads`; subtitle `Qualify new enquiries and turn them into deals`; `Add lead` button.
- Add-lead form, initially hidden:
  - Name text input (placeholder `Rahul Deshmukh`).
  - Company text input (placeholder `Company name`).
  - Source dropdown: Website, Referral, LinkedIn, Trade show, Cold call.
  - Expected value (₹) numeric input, minimum 0, placeholder `450000`.
  - `Save lead`, `Cancel`, inline validation/error message.
- Main card:
  - Status filter chips: All, New, Contacted, Qualified, Converted, Lost. Active chip is blue.
  - Search input, placeholder `Search name or company`; filters by name or company, case-insensitive.
  - Horizontally scrollable table columns: Lead (initials avatar, name, company), Source, Score (bar and number), Value, Status (pill), action.
  - Status pill styles: New = white bordered; Contacted = tinted blue; Qualified = blue; Converted = green; Lost = panel/muted.
  - `Convert to deal` appears except for Converted and Lost leads. Conversion changes lead status to Converted, creates a same-company deal at Qualified using lead value and owner, refreshes all calculated views, and shows `Converted to a deal in Qualified`.
  - Empty filtered state: `No leads match.`
- Add lead opens form and focuses Name. Saving requires non-empty name and company; otherwise show `Enter a name and company`. A valid lead is inserted with selected source, New status, score 60, entered positive value or zero, and owner `AK`; clear/reset form and filters, refresh the app, and show `Lead added`. Cancel closes form and clears the error.

## Screen 3 — Pipeline (`scrPipeline`)

- Header: `Deals pipeline`.
- Summary line: number of open deals, open value, and weighted forecast.
- Horizontally scrollable four-column board:
  1. Qualified — 30% chance
  2. Proposal — 50% chance
  3. Negotiation — 75% chance
  4. Won — 100% chance
- Each column shows stage name, deal count, total value, and chance. Empty state per column: `No deals here.`
- Deal cards show deal name, company, formatted value, owner initials avatar, and `Back` / `Next` buttons. Back/Next moves one stage while clamping to the first/last stage; boundary buttons are disabled. Advancing into Won shows `Deal won: {deal name}`.

## Screen 4 — Contacts (`scrContacts`)

- Header: `Contacts`; subtitle `People you sell to`.
- Search input, placeholder `Search by name or company`, case-insensitive.
- Responsive card grid (minimum 250 px card width). Each card shows initials avatar, name, job title, company, email, phone number, and `Last contact: {relative date}`.
- Empty state: `No contacts match.`
- Source behavior contains no add/edit contact action.

## Screen 5 — Activities (`scrActivities`)

- Header: `Activities`; subtitle `Calls, meetings, emails and tasks`; `Add activity` button.
- Add-activity form, initially hidden:
  - What needs doing text input (placeholder `Follow up on proposal`).
  - Type dropdown: Call (`call`), Meeting (`meet`), Email (`mail`), Task (`task`).
  - When dropdown: Today, Tomorrow, This week.
  - `Save activity`, `Cancel`, inline validation/error message.
- Main card:
  - Type filter chips: All, Calls, Meetings, Emails, Tasks.
  - Activities grouped under Today, Tomorrow, This week, omitting empty groups. Each row has completion checkbox, type icon, title, company, and time. Completed rows strike through and mute the title.
  - Empty state: `Nothing here.`
- Checkbox toggles completion and refreshes the dashboard and activity views.
- Add activity opens the form and focuses its title input. Saving requires a non-empty title; otherwise show `Describe the activity`. Valid save adds an incomplete activity using selected type/date, company `General`, and time `Anytime`; clear title/error, close form, reset filter to All, refresh, and show `Activity added`. Cancel closes form and clears the error.

## Seed data

Initialize local collections once at app start; no SharePoint or Dataverse sources.

### Leads (`colLeads`)

| Name | Company | Source | Status | Score | Value | Owner |
|---|---|---|---|---:|---:|---|
| Rahul Deshmukh | Sahyadri Foods | Website | New | 72 | 450000 | AK |
| Priya Kulkarni | Godavari Logistics | Referral | Contacted | 81 | 820000 | RM |
| Amit Joshi | Tapovan Textiles | LinkedIn | Qualified | 90 | 1250000 | AK |
| Neha Pawar | Panchavati Hotels | Trade show | New | 55 | 300000 | SP |
| Sagar More | Ganga Pharma | Cold call | Contacted | 48 | 210000 | RM |
| Kavita Shinde | Anjaneri Agro | Referral | Qualified | 86 | 960000 | SP |
| Vishal Gaikwad | Trimbak Steel | Website | Lost | 30 | 150000 | AK |
| Meera Jadhav | Kalwan Dairy | LinkedIn | Converted | 88 | 540000 | RM |

### Deals (`colDeals`)

Stage indices in the source map to Qualified, Proposal, Negotiation, Won.

| Name | Company | Value | Stage | Owner |
|---|---|---:|---|---|
| ERP rollout | Tapovan Textiles | 1250000 | Qualified | AK |
| Fleet tracking | Godavari Logistics | 820000 | Proposal | RM |
| Cold storage software | Sahyadri Foods | 640000 | Proposal | SP |
| Billing automation | Anjaneri Agro | 960000 | Negotiation | SP |
| HR portal | Ganga Pharma | 380000 | Negotiation | RM |
| POS licences | Panchavati Hotels | 520000 | Won | AK |
| Dairy ERP | Kalwan Dairy | 540000 | Qualified | RM |

### Contacts (`colContacts`)

| Name | Job title | Company | Email | Phone | Last contact |
|---|---|---|---|---|---|
| Rahul Deshmukh | Operations head | Sahyadri Foods | rahul@sahyadrifoods.example | 98220 11001 | 2 days ago |
| Priya Kulkarni | Fleet manager | Godavari Logistics | priya@godavarilog.example | 98220 11002 | Today |
| Amit Joshi | Managing director | Tapovan Textiles | amit@tapovantex.example | 98220 11003 | Yesterday |
| Neha Pawar | General manager | Panchavati Hotels | neha@panchavati.example | 98220 11004 | 5 days ago |
| Kavita Shinde | Finance lead | Anjaneri Agro | kavita@anjaneri.example | 98220 11005 | 3 days ago |
| Sagar More | IT manager | Ganga Pharma | sagar@gangapharma.example | 98220 11006 | 1 week ago |
| Meera Jadhav | Owner | Kalwan Dairy | meera@kalwandairy.example | 98220 11007 | Today |
| Vishal Gaikwad | Purchase head | Trimbak Steel | vishal@trimbaksteel.example | 98220 11008 | 3 weeks ago |

### Activities (`colActivities`)

`Done` is false except for the revised quote activity.

| Type | Activity | Company | Group | Time | Done |
|---|---|---|---|---|---|
| call | Intro call with Rahul | Sahyadri Foods | Today | 11:00 AM | No |
| meet | Proposal walkthrough | Godavari Logistics | Today | 3:00 PM | No |
| mail | Send revised quote | Anjaneri Agro | Today | 5:00 PM | Yes |
| call | Check contract terms | Tapovan Textiles | Tomorrow | 10:30 AM | No |
| task | Prepare demo environment | Ganga Pharma | Tomorrow | 2:00 PM | No |
| meet | Site visit | Panchavati Hotels | This week | Thu | No |
| mail | Share case study | Kalwan Dairy | This week | Fri | No |

## Required Power Apps implementation

- Native tablet canvas app and Power Fx only; screen/control names follow `scr...`, `con...`, `lbl...`, `btn...`, `gal...`, `ico...`.
- `App.Formulas` defines all shared colors as named formulas. `App.OnStart` initializes the four collections with the rows above. `App.StartScreen` selects Dashboard.
- Use containers and responsive `Parent.Width` / `Parent.Height` formulas. Use native Radius and DropShadow properties where supported.
- Keep all five screens and all listed controls, data, filters, validation, stage progression, completion toggles, dashboard calculations, empty states, and success feedback in parity with the source.
- The source requests Inter, standalone SVG icons, and a 1366 × 768 tablet design. If Inter is not available in the player, use the closest clean sans-serif and report that deviation.

# Canvas app implementation bindings

Revision: 1

Inputs:
- Approved discovery: D:\CRM\app\create-discovery.md (Revision: 1)
- D:\CRM\spec.md
- D:\CRM\app\create-design.md (Revision: 1)
- D:\CRM\app\S1.composition.md through D:\CRM\app\S5.composition.md (Revision: 1 each)

## Screen ownership

| Logical screen | YAML key | File | Prefix | Composition |
|---|---|---|---|---|
| S1 Dashboard | Screen1 | D:\CRM\app\canvas\Screen1.pa.yaml | `Dash` | D:\CRM\app\S1.composition.md@1 |
| S2 Leads | scrLeads | D:\CRM\app\canvas\scrLeads.pa.yaml | `Leads` | D:\CRM\app\S2.composition.md@1 |
| S3 Pipeline | scrPipeline | D:\CRM\app\canvas\scrPipeline.pa.yaml | `Pipe` | D:\CRM\app\S3.composition.md@1 |
| S4 Contacts | scrContacts | D:\CRM\app\canvas\scrContacts.pa.yaml | `Contacts` | D:\CRM\app\S4.composition.md@1 |
| S5 Activities | scrActivities | D:\CRM\app\canvas\scrActivities.pa.yaml | `Activities` | D:\CRM\app\S5.composition.md@1 |

## Data and state

- `colLeads`: `LeadId` Text, `Name` Text, `Company` Text, `Source` Text, `Status` Text, `Score` Number, `Value` Number, `Owner` Text. Seed rows are the 8 HTML leads.
- `colDeals`: `DealId` Text, `Name` Text, `Company` Text, `Value` Number, `Stage` Text, `Owner` Text. Seed rows are the 7 HTML deals. Stage probabilities are Qualified 0.30, Proposal 0.50, Negotiation 0.75, Won 1.00.
- `colContacts`: `ContactId` Text, `Name` Text, `Title` Text, `Company` Text, `Email` Text, `Phone` Text, `LastContact` Text. Seed rows are the 8 HTML contacts.
- `colActivities`: `ActivityId` Text, `Type` Text, `Title` Text, `Company` Text, `DueGroup` Text, `Time` Text, `Done` Boolean. Seed rows are the 7 HTML activities.
- Source: in-memory collections only. No connectors or data sources. Changes last for the current app run; they reset when `App.OnStart` runs again.
- App-level filter variables: `varLeadFilter`, `varLeadSearch`, `varContactSearch`, `varActivityFilter`; entry/feedback state: `varLeadFormOpen`, `varLeadError`, `varActivityFormOpen`, `varActivityError`, `varToast`, `varNavKey`.
- Created lead/activity IDs use `Text(GUID())`. Stage and completion actions resolve rows by those IDs. Lead conversion writes Converted and adds a Qualified deal with same company, value and owner.
- Initial dataset bounds: 8 leads, 7 deals, 8 contacts, 7 activities. User-created leads and activities are uncapped; the presentation does not assume a fixed total.

## Shared presentation

- Named formulas in `App.Formulas`: `cBlue`, `cLightBlue`, `cNavy`, `cTeal`, `cGreen`, `cText`, `cMuted`, `cBorder`, `cPanel`, `cTint`, `cWhite`, `cFont`, `cAccentSvg`, `cOpenKpiSvg`, and `FmtINR` (lakh formatting with Indian comma grouping below one lakh).
- Wide layout breakpoint: `App.Width < 900`. Wide layout has a 98 px navy rail and content starting at x=124; narrow layout hides the rail and presents a horizontal gallery navigation row with touch scrolling.
- A 5 px SVG blue-to-teal strip spans the top. White cards use border, native shadow and 14 px corner radius; pipeline columns use panel fill and 16 px radius. Primary controls use blue and white.
- `Segoe UI` is used because Inter is not present in the exposed native font enum. Navigation glyphs use text symbols; individual HTML SVG navigation/type symbols are not all reproduced.
- Main layout uses native GroupContainer manual and auto layouts. The Dashboard and Activities pages scroll vertically beneath fixed navigation; the pipeline board scrolls horizontally. Lists use native galleries. No PCF, custom code, or external data source.

## Cross-screen actions

| Action | Source and destination | State/result binding |
|---|---|---|
| A1 Navigation | Any screen to one of five screens | Header destinations remain in order. `varNavKey` updates on screen entry and in the mobile navigation gallery. Dashboard is `App.StartScreen` (`Screen1`). |
| A2 Lead filter/search | Leads list | Status and case-insensitive name/company predicates combine; the empty label shows `No leads match.` |
| A3/A4 Add or cancel lead | Leads form and list | Required name/company validation; nonnegative value; new New lead score 60 and owner AK; successful add resets form/filter/search and shows `Lead added`; Cancel closes and clears the error. |
| A5 Convert lead | Leads to Pipeline/Dashboard derived views | Guard excludes Converted/Lost; stable LeadId update plus new Qualified deal; success toast `Converted to a deal in Qualified`. |
| A6 Move deal | Pipeline stage board | Stable DealId Patch moves one stage; Qualified Back and Won Next are disabled; transition into Won shows `Deal won: {name}`. |
| A7 Contact search | Contacts gallery | Case-insensitive name/company predicate; empty label shows `No contacts match.` |
| A8 Activity type filter | Activities grouped galleries | Type filter is applied within Today/Tomorrow/This week; each empty group is hidden; no results shows `Nothing here.` |
| A9 Activity completion | Activity gallery to Dashboard | Stable ActivityId update toggles Done; completed title is muted/struck through; button offers Reopen. |
| A10/A11 Add or cancel activity | Activity form and grouped list | Blank title validation; new incomplete activity uses selected type/group, company General and time Anytime; success clears/closes/resets filter and shows `Activity added`; Cancel clears and closes. |

## Build status and evidence

- App initialization, named formulas and all five screens are written.
- `compile_canvas` passes for all 7 YAML files in `D:\CRM\app\canvas` with no errors or warnings.
- Final sync returned 7 files. A fresh empty-folder round trip also returned `App.pa.yaml`, all five expected screen YAML files with meaningful controls, and `_EditorState.pa.yaml`.
- Expected screens and their visible leaf controls are defined in each current YAML file. Runtime rendering, focus behavior, pointer/keyboard operation and persistence across app restarts have not been observed in the player.
- Skill workflow provenance: `canvas-app` version 3.1.0, from `C:\Users\Hsuyambu\Downloads\voice-to-text\power-platform-skills\plugins\canvas-apps`; plugin source revision unavailable.

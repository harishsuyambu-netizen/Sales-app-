Revision: 1

Inputs:
- D:\CRM\app\create-screens.md (Revision: 1)
- D:\CRM\app\create-functionality.md (Revision: 1)
- D:\CRM\spec.md (Sales CRM Canvas App Specification)
- C:\Users\Hsuyambu\Downloads\voice-to-text\power-platform-skills\plugins\canvas-apps\agents\canvas-create-layout.md (layout workflow)

# Sales CRM — shared visual design

## Shared identity and hierarchy

The five work areas retain the exact requested names and order: Dashboard, Leads, Pipeline, Contacts, Activities. The persistent app identity is a white `CRM` logo tile in the navigation. Dashboard is the initial area. Navigation selection always identifies the visible area, and the same navigation treatment is used on all five screens. Within each screen, the page title and task-specific subtitle lead, then the primary task or summary, then supporting details. Do not add a generic dashboard or extra work areas.

## Source design tokens and layout constraints

Preserve the supplied relationships and use the shared source tokens:

- Tablet reference canvas: **1366 × 768**; soft canvas background; **Segoe UI**.
- Primary blue `#2563EB`; light blue `#60A5FA`; navy `#102A43`; teal `#0F766E`; won/converted green `#15803D`.
- Text `#172B4D`; muted text `#5B6B7E`; border `#D8E1ED`; panel `#EEF3F8`; tint `#E8F0FF`; background `#F6F8FC`; cards remain white.
- Pipeline stage accents: Qualified blue `#2563EB`, Proposal purple `#6D28D9`, Negotiation amber `#B45309`, and Won green `#15803D`, each on a corresponding light-tinted panel.
- At wide widths, a **98 px vertical navy sidebar** and **26 px horizontal content padding**; a **5 px blue-to-teal gradient strip** spans the top of every screen.
- White cards: **1 px border**, **14 px radius**, subtle shadow, **18 px padding**. Pipeline stage panels use panel fill and **16 px radius**; deal cards use white fill and **14 px radius**.
- Page titles **26 px bold**; body **14 px**; muted descriptions **13 px**; card titles **15 px semibold**; KPI values **28 px bold**.
- Buttons and chips are pill-shaped. Primary actions use primary blue with white text; inactive chips use white with a border; selected chips use blue with white text.
- Currency uses Indian lakh style (for example `₹4.5 L`); values below one lakh use Indian comma grouping.

Color is never the sole carrier of meaning: pair stage/status color with visible text, and pair activity completion styling with an exposed completion state. Use dark text on light surfaces for dense copy; reserve light blue and teal for accents/bars or sufficiently large marks rather than small text on white. Keep labels and secondary text legible against their surfaces. Keyboard focus remains visibly distinct from selected/filter state.

## Shared spacing and density

Use an 8 px rhythm: 8 px for tight label/value relationships, 12 px within compact rows, 16 px between related controls, 18 px card inset, 24 px between major sections, and the source-prescribed 26 px horizontal page inset. Keep record identity and required values together; do not shrink dense record details to force an otherwise crowded row. At 1366 × 768, prioritize page title, primary controls/summary, and the start of the main result or board before substantial scrolling. Main records use a readable medium density with clear row/card separation; dashboards intentionally use compact summary cards followed by two-column evidence sections.

Interactive targets should be comfortably reachable (aim for at least 44 px high where practical). Keep labels visible beside fields rather than relying on placeholders; placeholders are examples, not labels. Use concise text feedback adjacent to the affected workflow and the shared success message location below.

## Shared navigation, chrome, and feedback

At widths of 900 px and above, the 98 px sidebar remains on the left with the `CRM` tile and five destinations in this exact order: Dashboard, Leads, Pipeline, Contacts, Activities. Its navy surface carries white inactive items. The selected item is white with primary-blue text. Keep the 5 px gradient strip across the top edge of the full screen. Navigation is persistent while the page content changes; it does not scroll with a long list. On Activities, keep the navigation above the scrolling content viewport so it remains visible and interactive.

Below 900 px, replace the vertical sidebar with the specified horizontally scrolling navigation row. Preserve all five labels and their order; the row scrolls independently so no destination is clipped or made unreachable. The active item remains identifiable by its selected treatment and text, not color alone. The content area takes the remaining width and the vertical screen content reflows beneath the nav.

Success notifications appear bottom-center for about **2.2 seconds**, with readable text and assistive-technology announcement. They must not cover an action, field, or validation message the user still needs. Error text stays with its form and is announced; it is not shown only through red color. Screen-to-screen selection changes with the visible destination. No external data workflow or persistence behavior is implied by these visual decisions.

## Viewports and reflow

The primary design anchor is the source tablet **1366 × 768**. Use **375 × 812** and **320 × 720** as narrow-width reflow checks, not as limits on supported sizes. The supplied breakpoint is preserved: below 900 px, navigation becomes the horizontal scroll row and dashboard two-column sections stack. Other two-column areas also stack in reading order. Form fields stack at narrow widths. Filter chips wrap to additional lines while remaining in source order; search occupies the available row width. Contacts reduce to one card per row. The lead table and pipeline board retain their meaningful columns and use localized horizontal scrolling instead of hiding required fields or forcing a tiny font. Page content scrolls vertically when needed; no screen-level content is cut off at the narrow anchors. The navigation scroll and explicitly identified horizontal result/board regions are the only independent horizontal scroll owners.

## Screen-family differences

Dashboard is a summary-first, compact two-column evidence layout after the four KPI cards. Leads is a form-and-worklist screen with a wide, information-dense result table. Pipeline is a stage-comparison board with four horizontally ordered lanes. Contacts is a search-first, lookup-only card grid. Activities is a filter-and-group completion list, with its optional creation form taking priority while open. These differences are intentional; do not impose one generic card grid, one filter placement, or one scroll pattern on all screens.

Status: Ready
Artifact: D:\CRM\app\create-design.md (Revision: 1)
Diagnostics: None

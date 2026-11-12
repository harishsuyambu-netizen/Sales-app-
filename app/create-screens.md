Revision: 1

Inputs:
- C:\Users\Hsuyambu\Downloads\voice-to-text\power-platform-skills\plugins\canvas-apps\agents\canvas-create-screens.md (instruction: canvas CREATE screens scope and artifact rules)
- D:\CRM\app\create-functionality.md (Revision: 1)
- D:\CRM\spec.md (Sales CRM Canvas App Specification)

Landing screen: S1 — scrDashboard.

S1 — scrDashboard (Dashboard)
Purpose: Show the current sales health summary and direct the user into operational follow-up work.
Owned R/A: R1, R9, A1, A10, A9 (after secondary actions from dashboard-related refreshes), A2/A7/A8 indirectly when the user navigates from the dashboard into a filtered work area.
Information needed: live seeded lead, deal, contact, and activity collections; current open deals before Won; stage totals and win probabilities; lead-source counts; incomplete activities due Today; top open deals by value; conversion rate across all leads.
Navigation: Dashboard is the default visible area on app start. The persistent sidebar routes to scrLeads, scrPipeline, scrContacts, and scrActivities; returning to Dashboard from any screen preserves the previously selected task context in the other areas, while the dashboard itself resets only when the underlying collections or filters change.
Local states: dashboard summary values are derived from the current collection state; no separate form state is kept on this screen. If no activities are due Today, the empty state is "All clear for today.".
Representative round trips:
- Origin: app start. The user lands on Dashboard and sees summary KPIs, stage totals, lead-source bars, and top-value deals. Selecting Leads keeps the dashboard in the background and opens the lead worklist with its own filter/search context.
- Origin: Activities screen after marking the current Today activity complete. Returning to Dashboard restores the refreshed overview with the activity count reduced and the updated top-of-day list.
- Origin: Leads screen after converting a lead into a Qualified deal. Returning to Dashboard shows the updated weighted forecast, new conversion count, and refreshed open-deal / source values.

S2 — scrLeads (Leads)
Purpose: Review, add, and convert sales leads while keeping the list within the current status and search context.
Owned R/A: R2, R3, R4, A2, A3, A4, A5, R9, A1.
Information needed: colLeads, current filter selection (All, New, Contacted, Qualified, Converted, Lost), current search string against name/company, lead value and owner, and the user-selected record when converting or reviewing.
Navigation: Accessed from the persistent sidebar or by returning from a converted lead flow. The lead entry form opens from this screen and closes back to the list without leaving the work area. Save success stays on Leads with the list refreshed and the filter reset to All; valid conversion lands on the lead list after the new Qualified deal is created.
Local states:
- Filter state: All/New/Contacted/Qualified/Converted/Lost; one selection active.
- Search state: case-insensitive name/company search; empty search means full current status subset.
- Form state: hidden by default; visible while adding a lead; validation message shown when name or company is missing; success message shown after a valid save; cancel clears the form and its error.
- Empty state: "No leads match." when the combined status + search result set is empty.
- Conversion state: only leads not in Converted or Lost can transition to a deal; conversion creates a same-company Qualified deal using the lead value and owner and marks the lead Converted.
Representative round trips:
- Origin: Dashboard. User selects Leads, sees the current lead list. They choose New status and search for "Sahy"; the result set narrows, and if nothing matches, the empty state replaces the list. Clearing the search returns the broader New filter result set.
- Origin: Leads list. User opens add-lead form, enters company only, saves, and sees the validation message "Enter a name and company" with no new record. Canceling the form closes it and returns to the list without a record.
- Origin: Leads list. User adds a valid lead with "AK" owner and value 0 or more; after save the form clears, filter resets to All, the new lead appears, and the dashboard and pipeline are refreshed. The success message "Lead added" is shown.
- Origin: Leads list. User selects a valid lead, chooses Convert to deal, and the same-company Qualified deal is created with the lead value and owner. The lead changes to Converted; subsequent return to Leads keeps the converted row visible under the Converted status filter and the dashboard/pipeline reflect the new deal.

S3 — scrPipeline (Pipeline)
Purpose: Review deal movement and stage health across the sales process.
Owned R/A: R5, A6, R9, A1.
Information needed: colDeals with each deal's company, owner, value, and stage; stage order Qualified, Proposal, Negotiation, Won; win chance by stage; open-deal totals and weighted forecast; and the selected deal when the user moves it Back or Next.
Navigation: This screen is reached from the sidebar. It is a natural destination from Dashboard for follow-up on open deals, and it returns to the current sales work area when the user leaves the app or navigates elsewhere. The selected deal and stage context stay relevant to the user until they change venue or move between stages.
Local states:
- Stage-board state: four columns, each with its own count, total value, and chance; a stage may be empty and shows "No deals here."
- Deal action state: each deal card offers Back and Next, with boundary buttons disabled at Qualified and Won so movement cannot exceed the allowed range.
- Transition state: moving a deal to Won triggers the specific feedback message "Deal won: {deal name}" and preserves the new stage in the board.
Representative round trips:
- Origin: Dashboard. User notices a value concentration in Negotiation and opens Pipeline to review the board. They move a Negotiation deal to Won; when the board returns, the deal now sits in the Won column and the feedback message appears.
- Origin: Pipeline. User attempts to move a Qualified deal backward; the button is disabled and the deal stays in Qualified. Moving it forward to Proposal changes the deal by one stage and retains the user on the same screen with the updated value and count totals.
- Origin: Pipeline. User moves the final deal in Proposal to Negotiation and then to Won; the board updates to the stage totals and weighted forecast, with the won message tied to the specific deal name.

S4 — scrContacts (Contacts)
Purpose: Search and inspect contact details for the people connected to leads and deals.
Owned R/A: R6, A7, R9, A1.
Information needed: colContacts including name, job title, company, email, phone, and last-contact recency; current search text applied case-insensitively to name and company.
Navigation: Reached from the sidebar. It is a lookup-only screen with no create or edit actions, so it returns the user to the current working context without changing records or filters in other areas.
Local states:
- Search state: matches name or company ignoring case.
- Empty state: "No contacts match." when the search yields no result.
- Result state: cards display the contact's initials, company, email, phone, and relative last-contact value.
Representative round trips:
- Origin: Dashboard or sales review. User searches for "panchavati" and sees the matching General Manager card. Clearing the search returns the full contact list.
- Origin: Leads screen after a lead is converted to a deal with a contact already in the system. User opens Contacts to verify the company person and then returns to the prior screen with the contact list still filtered to the relevant company or full list, depending on the query state.

S5 — scrActivities (Activities)
Purpose: Review each activity by due date and completion status and create new tasks without leaving the activity workflow.
Owned R/A: R7, R8, A8, A9, A10, A11, R9, A1.
Information needed: colActivities with type, title, company, due group, time, and completion flag; group order Today, Tomorrow, This week; current type chip selection (All, Calls, Meetings, Emails, Tasks); and the activity being created or toggled.
Navigation: Accessed from the sidebar. The add-activity form opens on this screen, closes back to the list, and resets filters after a successful save. Completing or reopening an activity refreshes the activity list and the dashboard overview because the Today count and task status can change.
Local states:
- Filter state: type chip selection; when set to a specific type, only matching activities remain visible.
- Grouping state: due-group headings appear only when their filtered results are non-empty; empty groups are omitted.
- Completion state: completed activities are muted and title text is struck through; toggling completion changes state and updates dependent information.
- Form state: hidden by default; visible while adding; validation message shown when title is empty; success message shown after valid save; cancel clears the form and error.
- Empty state: "Nothing here." when no activities remain after the selected type filter and due-group grouping.
Representative round trips:
- Origin: Dashboard. User selects Activities, filters to Calls, and sees only call entries grouped by Today/Tomorrow/This week. If no matching calls remain, the empty state appears instead of groups.
- Origin: Activities list. User marks an incomplete Today call complete; the row becomes muted and struck-through, and dashboard overview counts update accordingly. Reopening the same activity returns it to the active style and refreshes the dashboard totals.
- Origin: Activities list. User clicks Add activity, enters only whitespace or leaves the title blank, and sees "Describe the activity" without creating a record. Cancel closes the form with the error cleared. A valid save creates an incomplete activity for company "General" at "Anytime", closes the form, resets type filter to All, and shows "Activity added" while the new activity appears in the correct due group.

Cross-screen rationale: The requested five areas are intentionally split by user domain flow rather than by detail pages or generic settings. Dashboard owns summary health, Leads owns conversion and qualification workflow, Pipeline owns deal-stage movement, Contacts owns lookup-only reference data, and Activities owns task follow-up and completion. This grouping preserves a single work area per domain and keeps the navigation and refresh semantics tied to the life-cycle that created or changed the record, while leaving the underlying seeded collections local and in-memory.

Status: Ready
Artifact: D:\CRM\app\create-screens.md (Revision: 1)
Diagnostics: None

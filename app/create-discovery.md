# Canvas CREATE discovery

Revision: 1

Inputs:
- D:\CRM\spec.md (Sales CRM Canvas App Specification)
- D:\CRM\app\create-functionality.md (Revision: 1)
- D:\CRM\app\create-screens.md (Revision: 1)
- D:\CRM\app\create-design.md (Revision: 1)
- D:\CRM\app\S1.composition.md through D:\CRM\app\S5.composition.md (Revision: 1 each)
- Connected app: `04be591f-1f8c-48f5-b986-bec6f3f6e2f0`, environment `99636490-20be-e63d-bce7-6200ed9dc8de`

## Feasibility and resource decisions

- Canvas Authoring MCP connected successfully to the app session. Initial sync produced `App.pa.yaml`, `Screen1.pa.yaml`, and `_EditorState.pa.yaml`; blank app compilation passed before implementation.
- Available native controls include `GroupContainer`, `Gallery`, `Classic/Button`, `Classic/TextInput`, `Classic/DropDown`, `Label`, `Rectangle`, `Image`, and `Timer`. Direct control descriptions were read before configuration. Galleries support item templates; GroupContainer supports manual and auto layouts and rounded corners.
- `list_data_sources` returned zero data sources. This is consistent with the explicit request for no SharePoint or Dataverse. All four collections are local mock data seeded in `App.OnStart` from the HTML rows. New or changed rows remain in memory for the current app run and reset after a fresh app start; no backend, cross-device or durable storage is provided.
- Source data bounds at first load: 8 leads, 7 deals, 8 contacts and 7 activities. Leads and activities can grow through the requested add flows; no artificial caps are applied.
- Native Power Fx supports the requested filter, mutation, stage transition, completion, dashboard aggregate, and feedback behaviors. No PCF, connectors, or external code are used.
- The player control metadata does not expose Inter as a supported font choice. Use Segoe UI as the clean sans-serif fallback.
- The first screen remains the platform-provided `Screen1` key and is the Dashboard. This avoids leaving a blank starter screen and preserves `App.StartScreen = Screen1`; the user-facing title is `Sales dashboard`.
- Dynamic dashboard, source, score, pipeline, contact, and activity views are feasible using native galleries. Mobile navigation uses a horizontal gallery with hidden scrollbar and horizontal touch scrolling.

## Approved implementation scope

- Five destinations in the requested order: Dashboard, Leads, Pipeline, Contacts, Activities.
- Local collection-backed data, filters, validation, lead conversion, deal stage movement, activity completion, activity creation, empty states, calculated KPIs, and transient feedback as specified in `D:\CRM\spec.md`.
- Shared design uses the requested token formulas, navy rail, blue-to-teal SVG accent, white bordered cards, native rounded buttons/cards, and responsive layout formulas.
- Known fidelity limits to report: Segoe UI replaces Inter; nav symbols are text glyphs instead of the HTML's individual SVG symbol paths; several details use native-control approximations. The main canvas uses `Screen1` instead of a `scrDashboard` YAML key.

## Approval

Status: Approved
Revision tuple: 1, 1, 1, S1@1, S2@1, S3@1, S4@1, S5@1, 1
Evidence: The user's original request explicitly directs implementation of this standalone sales CRM, requires local HTML sample rows and native Power Fx, and says to continue without waiting after producing the specification.

# Changelog

## v1.1.17

- **Compact Tracking Layout** — status, priority, and updated date displayed in a single row on the tracking page; updated/resolved shown in secondary style on the right
- **Template Label Resolution** — Template resolver now supports field labels as placeholders (`{Summary}`, `{System / Service}`, etc.) in addition to field IDs
- **Field Label Uniqueness** — duplicate field labels are detected in the form editor with Ring UI ErrorBubble notification and red border; empty labels also flagged for non-display fields
- **Field Label Validation** — labels containing reserved characters (`{}`, `<>`, `` ` ``, `\`, `|`) show an error; all label errors displayed via Ring UI ErrorBubble
- **Demo Data Prefill** — "Prefill with demo data" button in admin UI (empty state) creates sample categories (IT Support, HR Requests) and forms (Bug Report, Access Request, Leave Request, Multi-Step Demo) for quick onboarding; reuses existing categories by title match; Multi-Step Demo showcases all 13 field types in a 2-step wizard
- **Orphan Forms Visibility** — forms with missing/deleted category now displayed in "Category not found" group instead of silently disappearing from admin UI
- **Submit Success UX** — after form submission, "Back to Portal" button navigates to catalog; browser back no longer shows blank page
- **Required Field Validation** — all field types (select, radio, checkbox, date, tags) now show ErrorBubble on submit when required but empty
- **Form Validation Styling** — inline error messages styled with Ring UI error color
- **Island Form Layout** — form view uses native Ring UI card styling with header border
- **Ring UI Panel** — form navigation buttons use native Ring UI panel; Back aligned left, Submit/Next aligned right
- **Catalog View Modes** — removed compact grid view; updated view mode icons
- **Tracking Fix** — fixed error when loading tracking page
- **Dead Code Cleanup** — removed some unused variables from track handler single-pass optimization
- **ValuesMap Collision Guard** — label-based keys in valuesMap are only added if not already occupied, preventing overwrite from duplicate labels
- **Tags Deduplication** — selected tags are now hidden from the dropdown; duplicate selection is prevented
- **Empty Fields in Description** — auto-generated and template descriptions now include all fields; empty values shown as "—"
- **State Reset Consolidation** — unified portal state reset into a single function, fixing stale state on browser back navigation
- **Summary Fallback** — if summary template resolves to empty string, falls back to form title
- **Boolean Fields Display** — checkbox values shown as "Yes"/"No" in descriptions instead of raw "true"/"false"
- **Field Mapping Type Safety** — Simple custom fields (string, integer, date, etc.) now filtered by subtype; incompatible mappings (e.g. string → date) no longer offered in editor
- **Required Label** — form editor checkbox label changed from abbreviated "req" to full "required"
- **Guest Mode: Enforced Fixed Reporter** — selecting Access=Guests automatically switches Reporter to Fixed login and disables the toggle; existing guest forms also enforce this on edit
- **Guest Banned Detection** — form editor checks if Guest user is globally banned in YouTrack; if so, the Guests option is disabled with a label and an error banner is shown
- **Guest Mode Documentation** — README updated with detailed Guest Mode setup guide, behavior description, and troubleshooting

## v1.0.117

- **Issue Description on Tracking** — tracking page now displays the issue description with lightweight markdown rendering (bold, horizontal rules, newlines)
- **Dynamic State Detection** — state field identified dynamically via `isResolved` property instead of hardcoded field name; works across all locales and custom field names
- **Dynamic Priority Detection** — priority field identified dynamically via `ordinal` property instead of hardcoded "Priority" field name
- **Description Generation Fix** — fields mapped to `__description` pseudo-target are no longer incorrectly excluded from auto-generated description
- **Tracking Resilience** — `loadTracking` wrapped in try/finally to prevent infinite loading state on network errors
- **Salt Consistency** — track handler now passes `installationId` to `getOrCreateTrackingSalt`, matching submit handler behavior
- **Form Migration** — added `formWidth` to `FORM_DEFAULTS` for consistent migration of older stored forms
- **Security: Access Control** — `/forms/get` and `/submit` endpoints now enforce `accessLevel` check on the backend; guests are denied access to `project_members` and `youtrack_users` forms even if they know the slug
- **Security: CSPRNG Salt** — tracking salt generation upgraded to `java.util.UUID.randomUUID()` (backed by `SecureRandom`) with `Math.random()` fallback for restricted sandboxes

## v1.0.59

- **Guest Tracking Page** — guests can track issue status via stateless deep-link token (`TR_<token>` in URL hash); tracking page shows summary, status, priority, created/updated/resolved dates; token generated with HMAC-SHA256 + installation salt
- **Submitted Fields on Tracking** — tracking page displays all visible form fields filled by the guest (label + value table); data stored in `issue.extensionProperties.sfcenter`; guest email excluded from display
- **HMAC-SHA256 tracking tokens** — replaced djb2 (31-bit) with HMAC-SHA256 (256-bit) for cryptographically secure tracking links; pure JS implementation ported from adv.attachments; 32-char random salt
- **DRY: parseStore optimization** — `getOrCreateTrackingSalt` now accepts pre-parsed store to avoid redundant JSON parse/stringify cycles during submit
- **Track status button** — success page shows "Track status" button for guests (deep-link navigation) and "View issue" link for members
- **Security fixes** — removed `description` and `projectName` from track response (information leak prevention); guest email excluded from `submittedFields`

## v1.0.39

- **Guest view (preview)** — toggle in dropdown menu on the portal to preview catalog and forms as a guest user; Ring UI warning Banner with icon
- **Guest email field** — automatic required email field for guest users on guest-accessible forms; appended to issue description on submit
- **Internal Project ID** — project internal ID (`internalProjectId`) is resolved and saved when editing forms in the constructor; portal uses it directly without calling project API (fixes guest 404 errors)
- **Navigation sync** — polling fallback for browser back/forward via `getAppLocation()` with `useRef` to prevent repeated form opens; `lastHashRef` synced on openForm/closeForm/deep-link
- **Form state protection** — `handleLocationChange` skips re-opening if the same form is already active, preventing form values/errors reset during polling
- **Ring UI search input** — portal search replaced with Ring UI `<Input>` with search icon and clear button
- **Auto-growing textarea** — Ring UI `<Input multiline>` without fixed rows for auto-expanding behavior
- **Breadcrumbs cleanup** — removed redundant "Portal Index" link, leaving only back arrow + active form title
- **Project ID resolution** — `resolveProjectId` uses admin endpoint with fallback to public search; skips API calls entirely when `internalProjectId` is available
- **Backend public form DTO** — `internalProjectId` included in public form response so portal can create issues without project API access

## v1.0.2

- **Wizard Step Editor** — new `step-break` field type to split wizard forms into steps; dedicated "Add Step Break" button in field list; step-break renders as a distinct darker block with badge
- **Wizard ↔ Single layout** — switching to single automatically removes all step-breaks; switching to wizard shows an info banner if no step-breaks exist
- **Steps conversion** — on save, flat field list with step-breaks converts to `steps[]` array; on load, `steps[]` converts back to flat list with step-break separators
- **Arrow stepper on portal** — step progress indicator with numbered badges, arrow separators, and 3 visual states (done/active/pending)
- **Portal navigation buttons** — Back button aligned left, Next/Submit aligned right
- **Form Editor layout** — Category, Layout, Width controls moved above Title for better workflow
- **Success page width** — submission confirmation now respects the form's width setting (compact/normal/wide)

## v0.9.235

- **Service Catalog** — public portal page with categories, form cards, search, and direct deep-link URLs
- **Form Engine** — schema-driven forms with 14 field types (string, textarea, select, multiselect, radio, checkbox, date, number, email, tags, hidden, heading, paragraph, banner)
- **Multi-step Wizard** — forms support single-page and wizard (multi-step) layouts with step progress indicator
- **Field Mapping** — map form fields to YouTrack custom fields (enum, date, text, etc.) with automatic type compatibility checks
- **Static Mapping** — set fixed values on created issues (e.g., Type=Bug, Priority=Critical)
- **Dynamic Options** — select/multiselect/radio fields can source options from YouTrack enum custom fields at runtime
- **Summary & Description Templates** — configurable templates with `{field_name}` placeholders for issue creation
- **Category Management** — create, edit, delete categories with duplicate-title protection and in-use checks
- **Form Editor** — inline editor with field reordering, type-specific settings, and live slug/deep-link preview
- **Project Settings Widget** — enable/disable service center per project, manage forms, publish/unpublish
- **Deep Linking** — direct URLs to specific forms via `host.navigation` API (YouTrack 2026.1.1+)
- **Form Width** — compact, normal, and wide layout options for forms

## v0.1.0

- **Initial scaffold** — project structure, build scripts, docs, and licenses adapted for Adv.ServiceCenter

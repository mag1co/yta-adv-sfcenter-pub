# Adv.ServiceCenter — Service Center / Forms Center for YouTrack

Unified portal for requests, surveys, and forms inside YouTrack — implemented as a native YouTrack App.

Works with YouTrack Server and Cloud.

## Why

Standard YouTrack helpdesk online forms are limited in UI flexibility and logic. Adv.ServiceCenter provides a full-featured service catalog with a schema-driven form engine, multi-step wizards, and field mapping — all running inside YouTrack as a native app.

## Features

- **Service Catalog** — portal page with categories, form cards, search, and direct deep-link URLs
- **Schema-Driven Forms** — 14 field types configured via admin UI, not code
- **Multi-step Wizard** — single-page and wizard layouts with step-break separators; arrow stepper with numbered badges and done/active/pending states
- **Wizard Step Editor** — `step-break` field type to split forms into steps; "Add Step Break" button; automatic flat ↔ steps conversion on save/load
- **Field Mapping** — map form fields to YouTrack custom fields with automatic type compatibility
- **Static Mapping** — set fixed values on created issues (e.g., Type=Bug, Priority=Critical)
- **Dynamic Options** — select/multiselect fields source options from YouTrack enum fields at runtime
- **Templates** — summary and description templates with `{field_name}` placeholders
- **Per-project Settings** — enable/disable service center, manage forms per project
- **Backend Issue Creation** — issues are created server-side via YouTrack entities API (no permanent tokens, no REST API from frontend)
- **Guest Access** — forms with `guests` access level allow unauthenticated users to submit requests; guest email is collected and stored in issue description
- **Guest Notifications** — if the guest's email matches a registered YouTrack user (LDAP/SSO/Hub), the user is automatically subscribed to issue updates
- **Guest Tracking Page** — after submission, guests receive a stateless tracking link (`TR_<token>`) to monitor issue status, description, priority, and dates; HMAC-SHA256 tokens ensure tamper-proof links
- **Markdown Description Rendering** — issue descriptions on the tracking page are rendered with lightweight markdown (bold, horizontal rules, newlines)
- **Dynamic Field Detection** — state and priority fields are identified dynamically by value properties (`isResolved`, `ordinal`), supporting any locale or custom field name
- **Smart Description Generation** — unmapped form fields are always included in the auto-generated issue description; mapped fields are included only when explicitly referenced in `descriptionTemplate`
- **Ring UI** — native YouTrack look and feel using JetBrains Ring UI components

## Guest Issue Creation — Limitations

> **After submission, a guest user does not have access to the created issue**, unless their email corresponds to a registered YouTrack user (e.g., via LDAP, SSO, or Hub).

- **Reporter** — issues created by guests have `guest` as the reporter (YouTrack system guest account)
- **Notifications** — only sent if the guest's email matches an existing YouTrack user; otherwise, the email is stored in the issue description for manual follow-up
- **User creation** — the app cannot create new YouTrack users (httpHandler sandbox blocks outgoing HTTP)
- **Issue tracking** — guests cannot view issues directly in YouTrack but can track status via the portal tracking page (deep-link with HMAC-SHA256 token)
- **User fields** — fields like Assignee cannot be set when the submitter is a guest (entities API permission limitation)

## Extension Points

| Extension Point | Purpose |
| --- | --- |
| `MAIN_MENU_ITEM` | Service Center portal page in sidebar |
| `PROJECT_SETTINGS` | Per-project form management and configuration |

## Requirements

- **YouTrack 2024.3+** — minimum version
- **YouTrack 2026.1.1+** — recommended for full functionality.
  Deep-link navigation (direct URLs to forms, browser back/forward support) uses the `host.navigation` API introduced in YouTrack 2026.1.1.
  On older versions the app still works, but deep-linking and back/forward navigation are unavailable.

## Tech Stack

- React 18 + TypeScript
- Ring UI (JetBrains component library)
- Host API (`host.fetchApp`, `host.fetchYouTrack`, `host.navigation`)
- Vite build

## Settings

Defined in `settings.json`:

| Setting | Type | Default | Description |
| --- | --- | --- | --- |
| Log Level | `enum` | `no_log` | Backend logging: `no_log`, `minimal`, `debug` |

---

## License

Proprietary. See [LICENSE](LICENSE) for terms.

## Vendor

- **Author**: mag1cc
- **Source**: [GitHub](https://github.com/mag1co/yta-adv-sfcenter-pub)

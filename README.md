# Adv.ServiceCenter — Service Center / Forms Center for YouTrack

Unified portal for requests, surveys, and forms inside YouTrack — implemented as a native YouTrack App.

Works with YouTrack Server and Cloud.

## Why

Standard YouTrack helpdesk online forms are limited in UI flexibility and logic. Adv.ServiceCenter provides a full-featured service catalog with a schema-driven form engine, multi-step wizards, and field mapping — all running inside YouTrack as a native app.

## Features

- **Service Catalog** — portal with categories, form cards, search, and deep-link URLs
- **Schema-Driven Forms** — 14 field types configured via admin UI, not code
- **Multi-step Wizard** — single-page and wizard layouts with arrow stepper
- **Field Mapping** — map form fields to YouTrack custom fields with type compatibility checks
- **Static Mapping** — set fixed values on created issues (e.g., Type=Bug, Priority=Critical)
- **Dynamic Options** — select/multiselect fields source options from YouTrack enum fields at runtime
- **Templates** — summary and description templates with `{Field Label}` placeholders
- **Per-project Settings** — enable/disable service center, manage forms per project
- **Backend Issue Creation** — issues created server-side, no permanent tokens needed
- **Guest Access** — guest-accessible forms with email collection and issue tracking
- **Guest Tracking Page** — stateless tracking links for guests to monitor issue status and updates
- **Demo Data Prefill** — one-click demo data with sample categories, forms, and a multi-step showcase
- **Form Validation** — required field checks with red borders and inline error messages
- **Catalog View Modes** — categories, grouped, and list views with dedicated icons
- **Ring UI** — native YouTrack look and feel using JetBrains Ring UI components

## Guest Mode — Setup & Limitations

### How to enable

1. In **Project Settings → Service Center**, open the form editor
2. Set **Access → Guests (external)**
3. Configure **Reporter**:
   - **Current user (default)** — issue is created as the Guest system user. Requires the YouTrack project to grant Guest permission to create issues.
   - **Fixed login** — issue is created on behalf of a specified user (e.g. a service bot). Use this when the project does not allow Guest to create issues directly. The specified user must exist in YouTrack and have permission to create issues in the target project.

### How it works

- An automatic **email** field is added to the top of the form
- User-type fields (Assignee, etc.) are hidden from mapping to protect sensitive data
- If the guest's email matches a registered YouTrack user, that user is subscribed to the issue (receives notifications)
- If no match is found, the email is stored in the issue description for manual follow-up
- Guests receive a **tracking link** (HMAC-SHA256 token) to monitor issue status without YouTrack access

### Limitations

- **No direct access** — after submission, a guest cannot view the issue in YouTrack (unless their email corresponds to a registered user via LDAP/SSO/Hub)
- **No user creation** — the app cannot create new YouTrack users (httpHandler sandbox blocks outgoing HTTP)
- **No user-field mapping** — Assignee, Verified by, etc. cannot be set when the submitter is a guest
- **Project permissions** — if Reporter is set to "Current user" but the project forbids Guest from creating issues, submission will fail with 403. Solution: switch to "Fixed login"

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

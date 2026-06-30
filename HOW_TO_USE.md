# How to Use Adv.ServiceCenter

## For Admins / Project Leads

> **Permissions:** Managing forms, categories, and titles requires the `UPDATE_PROJECT` permission. Project Admins have this by default. Each Project Admin sees and manages only the forms belonging to their own project.

### 1. Enable Service Center for a project

1. Open **Project Settings → Portal** tab
2. Click **Enable Service Center**
3. The project is now connected to the service catalog

### 2. Manage categories

- Click **+ Add Category** to create a new category (e.g., "IT Requests", "HR")
- Categories group forms in the catalog for easier navigation
- A category cannot be deleted if it has forms assigned to it

### 3. Create and configure forms

1. Click **+ Add Form** to create a new form
2. Fill in **Title** (a unique slug is generated automatically and used for deep-link URLs)
3. Choose **Layout**: Single page or Wizard (multi-step)
4. Choose **Form Width**: Compact, Normal, or Wide
5. Choose **Access Level** — controls who can see and submit the form:
   - **Project Members** (default) — only users with read access to the linked project
   - **Link Only** — hidden from the portal catalog; any logged-in YouTrack user can access the form via a direct URL (same access rules as "All YouTrack Users" but without catalog visibility)
   - **All YouTrack Users** — visible in catalog to all logged-in users; no membership check. _Option is disabled if no ACL group is configured in App Settings, or if you are not a member of the configured group._
   - **Guests (external)** — publicly accessible; requires email field; see Guest Mode section. _Option is disabled if no ACL group is configured in App Settings, or if you are not a member of the configured group._
6. Add fields — 14 types available:
   - **Input**: string, textarea, select, multiselect, radio, checkbox, date, number, email, tags, hidden
   - **Decorative**: heading, paragraph, banner
7. For select-like fields, choose between manual options or **Source** from YouTrack enum custom fields
8. Set up **Field Mapping** to map form fields to YouTrack issue fields
9. Set up **Static Mapping** to set fixed values on created issues (e.g., Type, Priority); for multi-value fields (`enum[*]`, `version[*]`, etc.) a multi-select picker appears automatically
10. Configure **Summary Template**, **Description Template**, and **Initial Comment** with `{Field Label}` placeholders — the editor shows a live list of available placeholders based on the current form fields; guest forms also show `{__guest_email}`

### 4. Multi-step Wizard

To create a multi-step form (wizard):

1. Set **Layout → Wizard** in the form editor
2. Add fields as usual, then click **+ Add Step Break** to split the form into steps
3. Each step break creates a new page — fields above the first break become Step 1, fields between breaks become Step 2, etc.
4. On the portal, users see an arrow stepper with numbered steps and navigate with **Back** / **Next** buttons

### 5. Dynamic Options

Select, multiselect, radio, and tags fields can load options from YouTrack custom fields at runtime:

1. In the field settings, set **Source** to a YouTrack enum custom field (e.g., Priority, Type)
2. Options are fetched when the form is opened on the portal
3. Manual options and source options cannot be used together — source overrides manual options

### 6. Reporter Mode

Controls who is set as the issue reporter on submission:

- **Current user** (default) — the logged-in user who submits the form becomes the reporter
- **Fixed login** — a specified YouTrack user (e.g., a service bot) is set as the reporter. The user must exist and have permission to create issues in the target project

Configure in the form editor under **Field Mapping → Reporter**.

> **Note:** For Guest forms, Reporter is automatically set to Fixed login (guests cannot be issue reporters).

### 7. Form Width

Controls the maximum width of the form on the portal:

- **Compact** — narrow, suitable for simple forms with few fields
- **Normal** (default) — standard width
- **Wide** — full-width, suitable for forms with many fields or long text areas

### 8. Guest Field Options Filter

For forms with **Access → Guests**, you can restrict which options guests see in select/radio/tags fields:

1. Open the field settings in the form editor
2. Set **Include** or **Exclude** values for guest users
3. On the portal, guests only see the allowed options; logged-in users see all options

This setting is only visible when the form's access level is **Guests**.

### 9. Conditional Static Mappings

Static mappings can be marked as **Guest only** — they are applied only when a guest submits the form:

1. In the form editor, go to **Static Mappings**
2. Check **Guest only** next to the mapping
3. When a logged-in user submits, the mapping is skipped

This is useful for setting a specific Type or Priority only for guest submissions.

### 10. Guest Preview

Admins can preview how the portal looks for guest users:

1. Open **Service Portal** in the sidebar
2. Click the dropdown menu (⋮) and toggle **Guest preview**
3. The catalog and forms are shown as a guest would see them (with a warning banner)

### 11. Publish

- Click **Publish** to make the form visible in the portal
- Only published forms from enabled projects appear in the catalog
- Copy the deep link URL to share a direct link to the form

---

### 12. App Settings (global)

Go to **Administration → Apps → Adv.ServiceCenter → Settings** to configure global options:

| Setting | Description |
| --- | --- |
| **Log Level** | Backend logging verbosity (`no_log`, `minimal`, `debug`) |
| **ACL: Who can create 'All YT-Users' forms** | Select a YouTrack group. Only members can set `All YouTrack Users` access on forms. Leave empty to disable the option for everyone. |
| **ACL: Who can create 'Guests' forms** | Select a YouTrack group. Only members can set `Guests (external)` access on forms. Leave empty to disable the option for everyone. |

> **Note:** ACL settings take effect immediately. If a user is removed from the group, they can no longer save forms with the restricted access level (server-side check on save).

---

## For End Users

### Opening the portal

Click **Service Portal** in the YouTrack sidebar menu to open the catalog.

### Catalog view modes

The portal supports three view modes (toggle via icons in the toolbar):

- **Categories** — forms grouped by category with collapsible sections
- **Grouped** — forms grouped by project with category subgroups
- **List** — flat table of all forms, sortable by title

### Finding a form

- Browse by category or use the **search bar** to find forms by title
- Click a form card to open it

### Filling and submitting

1. Fill in the required fields (marked with asterisks)
2. For wizard forms, use **Next** / **Back** to navigate steps
3. Click **Submit** to create a YouTrack issue
4. A success message with the issue ID appears after submission

### My Requests

Authenticated users can view all issues they submitted through Service Center forms:

1. Open **Service Portal** in the sidebar
2. Click the dropdown menu (⋯) and select **My Requests**
3. The table shows:
   - **Summary** — clickable link to the issue in YouTrack
   - **Form** — name of the form used; shown as _(deleted)_ if the form no longer exists
   - **Created** — submission date formatted to your YouTrack locale
4. Click any column header to sort; newest requests are shown first by default

> **Note:** My Requests is not available to guest users.

---

### Tracking submitted requests (guests)

After submitting a form, guest users see a **Track status** button. Clicking it opens a tracking page that shows:

- Issue summary and description (with markdown formatting)
- Current status (colored badge: blue = active, green = resolved)
- Priority
- All submitted form field values
- Created / Updated / Resolved dates

The tracking link uses a secure HMAC-SHA256 token — it can be bookmarked and shared without exposing internal YouTrack data.

### Deep links

Forms have direct URLs. You can bookmark or share them — opening the link goes straight to the form.

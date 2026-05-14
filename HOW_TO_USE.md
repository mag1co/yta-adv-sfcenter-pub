# How to Use Adv.ServiceCenter

## For Admins / Project Leads

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
2. Fill in **Title** and **Slug** (URL-friendly ID for deep links)
3. Choose **Layout**: Single page or Wizard (multi-step)
4. Choose **Form Width**: Compact, Normal, or Wide
5. Add fields — 14 types available:
   - **Input**: string, textarea, select, multiselect, radio, checkbox, date, number, email, tags, hidden
   - **Decorative**: heading, paragraph, banner
6. For select-like fields, choose between manual options or **Source** from YouTrack enum custom fields
7. Set up **Field Mapping** to map form fields to YouTrack issue fields
8. Set up **Static Mapping** to set fixed values on created issues (e.g., Type, Priority)
9. Configure **Summary Template** and **Description Template** with `{field_name}` placeholders

### 4. Publish

- Click **Publish** to make the form visible in the portal
- Only published forms from enabled projects appear in the catalog
- Copy the deep link URL to share a direct link to the form

---

## For End Users

### Opening the portal

Click **Service Portal** in the YouTrack sidebar menu to open the catalog.

### Finding a form

- Browse by category or use the **search bar** to find forms by title
- Click a form card to open it

### Filling and submitting

1. Fill in the required fields (marked with asterisks)
2. For wizard forms, use **Next** / **Back** to navigate steps
3. Click **Submit** to create a YouTrack issue
4. A success message with the issue ID appears after submission

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

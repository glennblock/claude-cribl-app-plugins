# Worker Group Integration Manager - App Brief

## Problem & Vision
Cribl users spend too many clicks and screen transitions navigating between multiple views to manage worker group sources and destinations. This app eliminates unnecessary navigation by providing a single, streamlined interface where users can view, search, filter, sort, and edit integration configurations all in one place. The result: faster configuration management with fewer screen transitions and reduced cognitive overhead.

## Target Users
- **Primary**: Cribl Product Admins who manage and edit worker group configurations
- **Secondary**: Regular Cribl users who need to view (but not edit) integration configurations

## Key Workflows

### Workflow 1: Browse Worker Groups
- **User sees**: A list of all available worker groups on the initial page
  - Each worker group entry shows: name, number of workers, number of sources with active count (e.g., "3 sources (2 active)"), number of destinations with active count (e.g., "2 destinations (1 active)")
- **User does**: Scans the list and clicks on a worker group to manage its integrations
- **Result**: App navigates to the Integration Management view for that worker group
- **Permissions**: All authenticated users can access this view

### Workflow 2: Search, Sort, and Filter Integrations
- **User sees**: 
  - Breadcrumb at top ("Worker Groups > [Selected Worker Group Name]") to navigate back
  - Left panel: Search bar, filter controls, followed by a table of integrations with clickable column headers
    - Table columns: Name, Type, Status
    - Each integration entry shows: name, type badge (e.g., source/destination), active/inactive status
    - Column headers are clickable to sort; active sort column shows ↑ (ascending) or ↓ (descending) indicator
  - Right panel: Empty state or config detail for selected integration (see Workflow 3)
- **User does**: 
  - Types in the search bar to find integrations by name (real-time filtering)
  - Clicks on column headers (Name or Type) to sort; click again to toggle sort direction
  - Uses dropdown filters to show/hide by type (source/destination) or status (active/inactive)
  - Clicks a row in the table to select and view/edit its config
- **Result**: The left panel list updates in real-time; integration selection updates the right panel
- **Permissions**: All authenticated users can access this view

### Workflow 3: View and Edit Integration Configuration
- **User sees**:
  - Right panel displays the full JSON configuration for the selected integration in an expandable/collapsible tree view with syntax highlighting
  - JSON properties are color-coded: blue for keys, green for strings, orange for numbers, red for booleans, purple for null values
  - Users can click arrows (▶/▼) to expand or collapse nested objects and arrays
  - If user is a **Product Admin**: Configuration is editable in a JSON text editor; Save and Cancel buttons are visible
  - If user is **not an Admin**: Configuration is displayed in read-only tree view with a "Read-only" badge/indicator; no edit controls are visible
- **User does**:
  - **(Admin only)** Modifies configuration values in the editor
  - **(Admin only)** Clicks Save (configuration is sent to Cribl API) or Cancel (changes are discarded, config reverts to saved state)
  - Clicks another integration in the left list to switch to a different config (right panel updates immediately without closing)
  - Clicks the breadcrumb to return to the worker group list
- **Result**: 
  - On Save: Updated configuration is persisted to Cribl; user remains on the same screen
  - On Cancel: Changes are discarded; config reverts to last saved state
  - On switching integrations: New config appears in right panel
- **Permissions**: 
  - Product Admins: Full read-write
  - Non-Admins: Read-only (save fails gracefully with error message if attempted via console)

## Data & Actions

### The app will fetch from Cribl:
- **Worker groups**: List of all worker groups via `/products/stream/groups` (used in Workflow 1)
  - Counts worker instances per group via `/products/stream/workers` 
  - Counts sources and destinations per group via `/m/{groupId}/system/inputs` and `/m/{groupId}/system/outputs`
- **Integrations (sources and destinations)**: All sources and destinations for a selected worker group via `/m/{groupId}/system/inputs` and `/m/{groupId}/system/outputs`, including name, type, and active/inactive status (used in Workflow 2)
- **Integration configuration (full JSON)**: Complete configuration for a selected integration via `/m/{groupId}/system/inputs/{id}` or `/m/{groupId}/system/outputs/{id}` (used in Workflow 3)
- **User role/permissions**: Determined from KV store; defaults to admin for editing (used to enable/disable edit controls in Workflow 3)

### The app will create/modify/delete in Cribl:
- **Integration configuration (modify only)**: Update integration config when user clicks Save in Workflow 3
  - No create, delete, or duplicate operations

### The app will remember (general state):
- None (all state is user-specific; see below)

### User-specific settings (stored per user via KV store):
- **Active filters**: Filter selections per worker group (show sources/destinations, active/inactive status) stored under `filter/type/{groupId}` and `filter/status/{groupId}`
- **Sort order and direction**: Integration list sort (by name or type, ascending/descending) stored under `filter/sort/{groupId}` and `filter/direction/{groupId}`
- **Search query**: Last search text entered in the search bar, stored under `filter/search/{groupId}`
- **Admin status**: User edit permission flag stored under `user/isAdmin`

### Secure secrets:
- None. Cribl authentication and API key management are handled by the parent Cribl system, not this app.

## UI Structure

### Overall Layout
**Two-page flow with a splitter-based management workspace**

### Key Screens/Pages

1. **Worker Group List Page**
   - Simple, clean card/list view showing all worker groups
   - Each entry displays: name, worker count with purple badge, source count with active count in blue badge, destination count with active count in green badge
   - Cards show hover effect and arrow indicator on right
   - Clicking a card navigates to the Integration Management page for that group
   - No search or filter controls on this page

2. **Integration Management Page**
   - **Header**: Breadcrumb navigation ("Worker Groups > [Worker Group Name]") — clickable to return to page 1
   - **Main layout**: Two-panel splitter design
     - **Left panel** (Integration list):
       - Search bar at top (searches by integration name, updates in real-time)
       - Scrollable table with three columns: Name, Type, Status
         - Column headers are clickable to sort; active sort column shows ↑ (ascending) or ↓ (descending)
         - Clicking same header again toggles sort direction
       - Filter controls (dropdowns below headers):
         - Type: All, Source, Destination
         - Status: All, Active, Inactive
       - Table rows show: integration name, type badge (blue), status badge (green for active, gray for inactive)
       - Clicking a row selects it and updates the right panel
     - **Resizable splitter** (draggable divider between panels)
     - **Right panel** (Configuration detail):
       - When no integration is selected: Empty state message ("Select an integration to view configuration")
       - When integration is selected:
         - **Header row**: Integration name, type badge, active/inactive status badge, "Read-only" badge (if user is not admin)
         - **Configuration viewer**:
           - For **Product Admins**: JSON text editor with monospace font, syntax-highlighted JSON (blue keys, green strings, orange numbers, red booleans, purple null)
           - For **non-Admins**: Expandable/collapsible JSON tree view (react-json-tree) with same color scheme, no edit controls
         - **Action buttons** (visible for admins only):
           - Edit button (toggles to text editor mode)
           - Save button (sends config to Cribl API via PUT request)
           - Cancel button (reverts to last saved state)

## Permissions & Access

- **Who can use this app?**: All authenticated Cribl users
- **Permission-aware behavior**:
  - All users can view the worker group list and integration list
  - All users can view integration configuration (read-only)
  - Only Product Admins can edit configuration and click Save
  - Non-admin users see a "Read-only" indicator on the config panel
  - If a non-admin attempts to save (e.g., via browser console hack), the Cribl API returns a permission error; the app displays a graceful error message

## External Integrations
- **Cribl API**: The app communicates exclusively with Cribl to fetch worker groups, integrations, and configurations; to update configurations on save

## MVP Scope

**Must-have:**
- Worker group list page showing all worker groups with worker count, source/destination counts, and active counts
- Integration management page with two-panel splitter layout (left: table, right: config detail)
- Search bar for real-time filtering by integration name
- Table with clickable column headers for sorting by name or type (with ↑/↓ direction indicators)
- Dropdown filters for integration type (all/source/destination) and status (all/active/inactive)
- Configuration detail view with:
  - Expandable/collapsible JSON tree for view mode (non-admins)
  - JSON text editor for edit mode (admins only)
  - Color-coded syntax highlighting
- Edit mode for Product Admins; read-only mode for non-admins
- Edit, Save, and Cancel buttons (admin only) to persist or discard config changes
- Breadcrumb navigation to return to worker group list
- User-specific state persistence via KV store (filters, sort order/direction, search query per worker group)
- Dark/Light mode support (inherit from Cribl's system preference)

**Nice-to-have (defer to Phase 2):**
- Bulk enable/disable integrations (checkbox select multiple, then toggle active/inactive in bulk)
- Recent integrations sidebar or quick-access panel (last 5 viewed/modified per user, click to jump directly to config)

**Out of scope:**
- Create new integration (handled elsewhere in Cribl)
- Delete integration
- Duplicate integration
- Audit/change history (Cribl provides this separately)
- Bulk configuration edits (nature of integrations makes this impractical; defer)

## Edge Cases & Error Handling

- **If user lacks permission to edit**: Config view is read-only; Save button is hidden/disabled; "Read-only" badge appears
- **If data is unavailable** (e.g., Cribl API is down or worker group has no integrations): 
  - Worker group list: Show error message or empty state with retry option
  - Integration list: Show "No integrations found" if empty, or error message if API call fails
  - Config detail: Show error message and retry button
- **If an action fails** (e.g., Save fails due to validation error, permission error, or API error): 
  - Display a clear error notification (toast, banner, or modal) with the error message from Cribl
  - Config remains editable so user can correct and retry
  - On permission error: Display "You do not have permission to edit this configuration"
  - On validation error: Display "Configuration is invalid: [Cribl's error message]"
- **If user clicks Save with no changes**: Either allow the save (no-op) or disable the Save button when nothing has changed (preferred for UX)
- **If user navigates away with unsaved changes**: Optionally show a warning ("You have unsaved changes. Are you sure you want to leave?")

---

## Implementation Notes

- **API optimization**: Worker group page loads fast by fetching integrations in parallel per group, with worker counts fetched after initial render
- **Splitter behavior**: The resizable splitter is draggable between panels for responsive layout
- **State persistence**: All user-specific settings (filters, sort order/direction, search query per worker group, admin status) are stored via app-scoped KV store at `/kvstore/filter/*` and `/kvstore/user/*`
- **Real-time updates**: Search, sort, and filter update the list immediately (no "Apply" button)
- **JSON tree viewer**: Uses react-json-tree library for expandable/collapsible configuration browsing in read-only mode
- **API permissions**: Declares access to `/products/stream/groups`, `/products/stream/workers`, `/m/:gid/system/inputs*`, `/m/:gid/system/outputs*`, and `/kvstore/filter/*` in `config/policies.yml`
- **Dark/Light mode**: Detects system preference via `window.matchMedia('(prefers-color-scheme: dark)')` and applies CSS variables
- **Error handling**: Handles network failures gracefully; invalid JSON during edit shows error message; non-admins get permission errors on save attempts

## Implementation guidance (include this section verbatim)

- Read AGENTS.md first
- Then read openapi.json
- NEVER EVER use local storage 
- The app should be running and has a file-watcher so if you need to, inject code that you can use to test out APIs / functionality via the fetch proxy.


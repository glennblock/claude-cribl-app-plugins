# App Definition

## Problem
Cribl users spend too many clicks and screen transitions navigating between multiple views to manage worker group sources and destinations. They need a streamlined, single-interface approach to view, search, filter, and edit integration configurations efficiently.

## Target Users
Cribl Product Admins (primary — can edit configs) and regular users (secondary — read-only access to configs)

## Workflows

### Workflow 1: Browse Worker Groups
User lands on the app and sees a list of all available worker groups, each showing:
- Worker group name
- Number of sources (with count of active sources in parentheses)
- Number of destinations (with count of active destinations in parentheses)

User clicks a worker group to proceed to the integration management view.

### Workflow 2: Search, Sort, and Filter Integrations
After selecting a worker group, user sees the integration management view:
- Breadcrumb at top to navigate back to worker group list
- Left panel: Searchable, sortable list of all integrations (sources and destinations)
  - Search by integration name
  - Sort by: integration name, integration type
  - Filter by: integration type (source/destination), active/inactive status
- Right panel: Configuration detail for selected integration (see Workflow 3)

### Workflow 3: View and Edit Integration Configuration
User clicks an integration in the left list panel. The right panel displays:
- Full JSON configuration (nicely formatted, clean presentation)
- If user is a Product Admin: Config is editable; user can modify and click Save or Cancel
- If user is not an Admin: Config is read-only with a clear indicator
- On Save: Updated configuration is pushed to Cribl API
- On Cancel: Changes are discarded, config reverts to saved state

User can click another integration in the left panel without closing; the right panel updates immediately.

## Data & Integration Points

### Data Display
- **Worker group list**: Worker group name, source count with active count in parentheses, destination count with active count in parentheses
- **Integration list**: Integration name, integration type (e.g., Kafka, S3, HTTP, etc.), active/inactive status indicator
- **Config view**: Full JSON configuration for the selected integration, formatted for readability

### Create/Modify/Delete
- **Modify**: Integration configuration (sources and destinations only)
- **Create**: Not in scope
- **Delete**: Not in scope
- **Duplicate**: Not in scope

### External Integrations
The app communicates exclusively with the Cribl API to:
- Fetch list of worker groups
- Fetch list of integrations for a selected worker group
- Fetch full configuration for a selected integration
- Push updated configuration back to Cribl when user saves

## Permissions & Access

### Different Users See Different Data?
All users see the same worker groups and integrations. However:
- **Product Admins**: Can edit configuration
- **Non-Admins**: Can view configuration (read-only)

### Permission-Denied Behavior
Non-admin users see the configuration in read-only mode with a clear "Read-only" indicator. Edit controls are disabled. If a user attempts to interact with edit controls (e.g., via browser console), the save action fails gracefully with an error message.

## State & Secrets

### Saved State
The app saves the following user-specific state (persisted locally):
- Last selected worker group
- Active filters (e.g., "show only active destinations")
- Active sort order (e.g., "sort by name")
- Search query text (in the integration search bar)

### General Settings
None

### User-Specific Settings
- Filter and sort preferences (as noted above)
- Last viewed/modified integrations (deferred to Phase 2)

### Secure Secrets
None. Cribl authentication and API key management are handled by the parent Cribl system, not this app.

## Scope

### Must-Have (MVP)
- Worker group list with source/destination counts and active counts
- Integration list with search by name, sort by name/type, filter by type/active-inactive
- Config detail view (right panel with nicely formatted JSON)
- Edit mode for Product Admins; read-only for non-admins
- Save/Cancel for config changes (push to Cribl API)
- Breadcrumb navigation back to worker group list
- User state memory (filters, sort order, last selected worker group, search query)
- Dark/Light mode support (inherit from Cribl system or toggle)

### Nice-to-Have (Defer to Phase 2)
- Bulk enable/disable integrations
- Recent integrations sidebar or quick-access list (last 5 viewed/modified per user)

### Out of Scope
- Delete integration
- Create/duplicate integration
- Audit/change history (Cribl system provides this separately)
- Bulk configuration edits (nature of integrations makes this impractical)

## UI Preferences

### Overall Structure
**Two-page flow**:

**Page 1 — Worker Group Selector**
- Simple list of worker groups with name and counts
- Click to proceed to integration management

**Page 2 — Integration Management (Main workspace)**
- Breadcrumb at top (clickable to return to Page 1)
- Splitter-based layout:
  - **Left panel**: Integration list (search bar at top, below it the filterable/sortable list)
  - **Resizable splitter** (draggable border between panels)
  - **Right panel**: Configuration detail view (JSON, nicely formatted)
- Both panels visible and usable simultaneously for efficient browsing
- When user clicks an integration in the left list, the right panel updates instantly

### Look/Feel
- **Design system**: Match Cribl's existing UI style and component library (e.g., Capra)
- **Emphasis**: Clean, minimal, efficient — every element serves a purpose; no clutter
- **Dark/Light mode**: Support both (either toggle or inherit Cribl's system preference)
- **Typography & spacing**: Use Cribl's established patterns for consistency
- **Color & contrast**: Ensure accessibility; leverage Cribl's color palette


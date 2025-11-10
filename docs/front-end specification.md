# DnD Interactive Map Application UI/UX Specification

**Version:** 1.0  
**Date:** 2024-12-19  
**Status:** Draft

---

## Introduction

This document defines the user experience goals, information architecture, user flows, and visual design specifications for DnD Interactive Map Application's user interface. It serves as the foundation for visual design and frontend development, ensuring a cohesive and user-centered experience.

The application is a web-based interactive map tool for Dungeons & Dragons game masters, enabling them to manage campaigns, upload maps, place character tokens, and control fog of war. The design must prioritize the map as the focal point while keeping controls accessible and intuitive.

---

## Overall UX Goals & Principles

### Target User Personas

**Primary Persona: The Game Master (DM)**

- **Role:** Dungeon Master running D&D campaigns
- **Technical Comfort:** Moderate to high (comfortable with web apps)
- **Context:** Running sessions, needs quick access to maps and tokens during gameplay
- **Pain Points:** Switching between tools, losing token positions, managing multiple campaigns
- **Goals:** Quick map navigation, easy token placement, persistent campaign data
- **Primary Device:** Desktop (laptop/PC) - primary development target

**Secondary Persona: The Preparatory DM**

- **Role:** DM who prepares sessions in advance
- **Technical Comfort:** Varies
- **Context:** Planning sessions, organizing campaign materials
- **Pain Points:** Organizing maps and notes, setting up encounters
- **Goals:** Easy campaign organization, note-taking, character management

### Usability Goals

1. **Ease of Learning:** New DMs can upload a map and place their first token within 5-7 minutes (realistic onboarding time)
2. **Efficiency of Use:** Experienced DMs can switch maps and move tokens during active gameplay without breaking flow
3. **Error Prevention:** Clear confirmation dialogs for destructive actions (deleting campaigns, maps, characters)
4. **Memorability:** DMs returning after weeks can navigate to their campaign and maps without relearning
5. **Accessibility:** Keyboard navigation for all map interactions; screen reader support for campaign management
6. **Quick Character Access:** Character stats and attributes accessible via hover/click on tokens during movement

### Design Principles

1. **Map-First Design** - The map is the hero. UI elements should never obstruct the map view unnecessarily
2. **Progressive Disclosure** - Show only essential controls; advanced features accessible but not cluttered
3. **Immediate Visual Feedback** - Every action (token move, fog reveal, zoom) provides instant visual response
4. **Consistent Spatial Model** - Tokens maintain positions across sessions; grid system is predictable and reliable
5. **Touch & Mouse Parity** - All interactions work equally well with touch (tablets) and mouse (desktop), with desktop as primary
6. **Contextual Information** - Character stats and details accessible without leaving the map view (hover panels, side panels)

**Key Design Decisions:**

- **Desktop Primary:** Interface optimized for mouse/keyboard with larger screen real estate
- **Character Stats Integration:** Token interactions (hover/click) reveal character information overlay, allowing DMs to reference stats while managing tokens
- **Realistic Onboarding:** 5-7 minute learning curve accounts for account creation, campaign setup, and first map upload

---

## Information Architecture (IA)

### Site Map / Screen Inventory

```mermaid
graph TD
    A[Login/Register] --> B[Campaign Dashboard]
    B --> C[Campaign View]
    C --> C1[Map Viewer]
    C --> C2[Character Management]
    C --> C3[Campaign Notes]
    C --> C4[Campaign Settings]
    C1 --> C1A[Map Tools Panel]
    C1 --> C1B[Token Interaction]
    C1 --> C1C[Fog of War Tools]
    C2 --> C2A[Character List]
    C2 --> C2B[Character Editor]
    C2 --> C2C[Character Details]
    C3 --> C3A[Map Notes]
    C3 --> C3B[Campaign Notes]
    B --> B1[Create Campaign]
    B --> B2[User Settings]
```

**Screen Breakdown:**

- **Login/Register:** Authentication entry point
- **Campaign Dashboard:** Overview of all user campaigns
- **Campaign View:** Main workspace with collapsible sidebar navigation
- **Map Viewer:** Full-screen map with fixed top toolbar
- **Character Management:** CRUD operations for characters/NPCs/enemies
- **Notes Panel:** Campaign and map-specific notes

### Navigation Structure

**Primary Navigation (Campaign View):**

- **Top Bar:** Campaign name, user menu, logout
- **Left Sidebar (Collapsible):**
  - Toggle button (hamburger/chevron icon)
  - Maps list (with thumbnails when expanded)
  - Characters/NPCs/Enemies list (filterable when expanded)
  - Campaign Notes
  - Campaign Settings
  - When collapsed: Icon-only mode with tooltips on hover

**Map Viewer Navigation:**

- **Fixed Top Toolbar:**
  - Grid toggle button
  - Fog of War tools (draw/erase toggle)
  - Zoom controls (+/-/reset)
  - Character selector dropdown
  - Map settings icon
  - Sidebar toggle (to access maps/characters list)
- **Character Stats Panel (Right Side):**
  - Slides in from right on token click
  - Shows character name, HP, AC, stats, notes
  - Close button to dismiss
  - Clicking another token updates the panel

**Secondary Navigation:**

- **Breadcrumbs:** Dashboard > Campaign Name > Current Map (when in map viewer)
- **Contextual Panels:**
  - Character stats panel (slides in from right on token click)
  - Map notes panel (collapsible overlay)

**Breadcrumb Strategy:**

- Show full path when in nested views (Map Viewer, Character Editor)
- Hide breadcrumbs in Campaign Dashboard (top-level)
- Clickable breadcrumbs for quick navigation

**Key Navigation Decisions:**

- **Collapsible Sidebar:** Can be hidden during active gameplay to maximize map space; icon-only mode when collapsed
- **Fixed Top Toolbar:** Always visible, doesn't obstruct map view, provides consistent access to map tools
- **Click-Based Character Stats:** Token click opens character stats panel; prevents accidental reveals during token movement

---

## User Flows

### User Flow: First-Time User Onboarding

**User Goal:** Create account, set up first campaign, upload a map, and add first character to campaign

**Entry Points:** Landing page, login screen

**Success Criteria:** User has an active campaign with one map and one character added to the campaign

**Flow Diagram:**

```mermaid
graph TD
    A[Landing Page] --> B[Click Register]
    B --> C[Enter Email/Password]
    C --> D[Account Created]
    D --> E[Campaign Dashboard - Empty]
    E --> F[Click Create Campaign]
    F --> G[Enter Campaign Name/Description]
    G --> H[Campaign Created]
    H --> I[Campaign View Opens]
    I --> J[Click Upload Map]
    J --> K[Select Image File]
    K --> L[Map Uploaded]
    L --> M[Map Viewer Opens]
    M --> N[Click Add Character]
    N --> O[Character Entry Form Opens]
    O --> P[Enter Character Details from Sheet]
    P --> Q[Save Character to Campaign]
    Q --> R[Select Character from Dropdown]
    R --> S[Click on Map to Place Token]
    S --> T[Token Placed Successfully]
```

**Edge Cases & Error Handling:**

- Invalid email format → Show inline validation error
- Weak password → Show password requirements
- File upload fails → Show error message with retry option
- Image too large → Show file size limit message
- Network error during save → Show connection status, auto-retry
- Character data entry incomplete → Highlight required fields, prevent save

**Notes:**

- Character management is about adding existing character sheets to the campaign, not creating characters from scratch
- DMs receive character sheets from players and enter the data into the system
- This flow should be completable in 5-7 minutes
- Consider adding a brief tutorial tooltip overlay for first-time users

---

### User Flow: Active Gameplay - Moving Tokens

**User Goal:** Move character tokens on map during active D&D session

**Entry Points:** Map viewer with existing tokens

**Success Criteria:** Token moved to new position, position saved automatically, undo available if needed

**Flow Diagram:**

```mermaid
graph TD
    A[Map Viewer Open] --> B{Token Already on Map?}
    B -->|Yes| C[Click and Drag Token]
    B -->|No| D[Select Character from Dropdown]
    D --> E[Click on Map]
    E --> F[Token Placed]
    F --> C
    C --> G[Token Follows Mouse]
    G --> H[Release Mouse]
    H --> I{Snap to Grid Enabled?}
    I -->|Yes| J[Token Snaps to Grid]
    I -->|No| K[Token at Exact Position]
    J --> L[Position Auto-Saved]
    K --> L
    L --> M[Visual Feedback: Token Settled]
    M --> N[Undo Available in Toolbar]
    N --> O{User Wants Undo?}
    O -->|Yes| P[Click Undo]
    P --> Q[Token Returns to Previous Position]
    O -->|No| R[Continue Gameplay]
```

**Edge Cases & Error Handling:**

- Token dragged off map → Constrain to map boundaries
- Multiple tokens overlap → Show z-index stacking, allow reordering
- Network disconnection → Queue position updates, sync when reconnected
- Token click during drag → Cancel drag, open character stats panel
- Undo stack limit → Store last 10-20 moves, show indicator when limit reached

**Notes:**

- Movement should feel smooth (60fps)
- Grid snapping should be visually obvious
- Auto-save should be silent (no confirmation dialogs)
- Undo/redo feature included for gameplay flexibility - allows quick corrections during active sessions
- Undo stack should be per-session (not persisted across page reloads)

---

### User Flow: Managing Fog of War

**User Goal:** Reveal hidden areas of map by erasing fog of war

**Entry Points:** Map viewer with fog of war enabled

**Success Criteria:** Fog erased, area revealed, change persisted immediately

**Flow Diagram:**

```mermaid
graph TD
    A[Map Viewer] --> B[Click Fog of War Tool]
    B --> C[Toolbar Shows Draw/Erase Options]
    C --> D[Select Erase Mode]
    D --> E[Click and Drag on Map]
    E --> F[Fog Erased in Real-Time]
    F --> G[Release Mouse]
    G --> H[Fog Change Saved Immediately]
    H --> I[Tokens in Revealed Area Become Visible]
```

**Edge Cases & Error Handling:**

- Drawing fog over existing tokens → Tokens become hidden
- Erasing large areas → Show progress indicator for save operation
- Rapid fog changes → Batch save operations to prevent performance issues

**Notes:**

- Fog drawing should feel responsive and immediate
- Changes commit immediately (no preview mode needed)
- Consider brush size options for different reveal areas
- Real-time erase provides immediate feedback for active gameplay

---

### User Flow: Adding Character to Campaign

**User Goal:** Add a player's character to the campaign using their character sheet

**Entry Points:** Campaign view, character management panel

**Success Criteria:** Character added with all relevant stats and information, available for token placement

**Flow Diagram:**

```mermaid
graph TD
    A[Campaign View] --> B[Open Character Management Panel]
    B --> C[Click Add Character Button]
    C --> D[Character Entry Form Opens]
    D --> E[Enter Character Name]
    E --> F[Select Character Type: PC/NPC/Enemy]
    F --> G[Enter Character Stats from Sheet]
    G --> H[HP, AC, Ability Scores]
    H --> I[Upload Token Image Optional]
    I --> J[Add Character Notes]
    J --> K[Save Character]
    K --> L[Character Added to Campaign]
    L --> M[Character Appears in Character List]
    M --> N[Available for Token Placement]
```

**Edge Cases & Error Handling:**

- Missing required fields → Highlight and prevent save
- Invalid stat values → Show validation (e.g., AC range, HP positive)
- Duplicate character name → Warn but allow (characters can have same name)
- Token image upload fails → Use default token icon, allow retry

**Notes:**

- Character entry is data entry from existing character sheets, not character creation
- Form should be efficient for quick data entry
- Consider character templates for common NPC types (future enhancement)
- Token image is optional - system can provide default icons

---

**Key Flow Decisions:**

- **Undo/Redo for Token Movement:** Included for gameplay flexibility - allows quick corrections during active sessions without breaking flow
- **No Preview Mode for Fog:** Changes commit immediately for responsive gameplay experience
- **Character Management:** Focused on adding existing character data to campaigns, not character creation from scratch

---

## Wireframes & Mockups

**Primary Design Files:** To be created in Figma/Sketch (TBD)

**Note:** Detailed visual designs will be created in design tools. Below are conceptual layouts for key screens to guide the design process.

### Key Screen Layouts

#### Campaign Dashboard

**Purpose:** Overview of all user campaigns, entry point after login

**Key Elements:**

- Header with user menu and logout
- "Create New Campaign" button (prominent, top-right)
- Grid/list view of campaign cards
- Each campaign card shows:
  - Campaign name
  - Description (truncated)
  - Last modified date
  - Number of maps
  - Number of characters
  - Thumbnail/preview image (optional)
- Empty state: Friendly message with "Create Your First Campaign" CTA

**Interaction Notes:**

- Click campaign card → Navigate to Campaign View
- Hover on card → Show quick actions (edit, delete)
- Responsive grid: 3-4 columns on desktop, 2 on tablet, 1 on mobile

**Design File Reference:** `campaign-dashboard.figma` (to be created)

---

#### Campaign View (Main Workspace)

**Purpose:** Central hub for campaign management with sidebar navigation

**Key Elements:**

- **Top Bar:**
  - Campaign name (editable)
  - Breadcrumb: Dashboard > Campaign Name
  - User menu, logout
- **Left Sidebar (Collapsible, ~250px when expanded):**
  - Toggle button (hamburger icon)
  - Maps section:
    - "Upload Map" button
    - Scrollable list of maps with thumbnails
    - Click map → Opens in Map Viewer
  - Characters section:
    - "Add Character" button
    - Filter tabs: All / PCs / NPCs / Enemies
    - Scrollable list of characters
    - Click character → Opens character details/edit
  - Campaign Notes section
  - Campaign Settings section
- **Main Content Area:**
  - Default: Campaign overview or selected map
  - Shows map viewer when map is selected
  - Shows character management panel when in character mode

**Interaction Notes:**

- Sidebar collapse/expand animation (smooth slide)
- When collapsed: Icon-only mode with tooltips
- Main content area resizes dynamically based on sidebar state

**Design File Reference:** `campaign-view.figma` (to be created)

---

#### Map Viewer

**Purpose:** Full-screen interactive map display with token placement and fog of war

**Key Elements:**

- **Fixed Top Toolbar:**
  - Grid toggle (on/off)
  - Fog of War tools (draw/erase toggle, brush size)
  - Zoom controls: Zoom In, Zoom Out, Reset, Zoom level indicator
  - Character selector dropdown (for placing new tokens)
  - Undo/Redo buttons
  - Map settings icon
  - Sidebar toggle button
- **Map Canvas Area:**
  - Full viewport minus toolbar height
  - Click and drag to pan
  - Mouse wheel to zoom
  - Grid overlay (when enabled)
  - Tokens (draggable, clickable)
  - Fog of war layers
- **Character Stats Panel (Right Side, Slides In):**
  - Triggered by token click
  - Shows character name, type
  - HP, AC, ability scores
  - Character notes
  - Edit button
  - Close button (X)
  - Width: ~300px

**Interaction Notes:**

- Map fills available space (responsive to viewport)
- Toolbar always visible, doesn't scroll with map
- Character stats panel overlays map, can be dismissed
- Tokens snap to grid when grid enabled
- Smooth zoom/pan interactions

**Design File Reference:** `map-viewer.figma` (to be created)

---

#### Character Entry Form

**Purpose:** Add character to campaign by entering data from character sheet

**Key Elements:**

- Modal or slide-in panel
- Form fields:
  - Character name (required)
  - Character type: Radio buttons (PC / NPC / Enemy)
  - Token image upload (optional, with preview)
  - Stats section:
    - HP (number input)
    - AC (number input)
    - Ability scores: STR, DEX, CON, INT, WIS, CHA (number inputs)
  - Notes (textarea)
- Action buttons:
  - Save (primary)
  - Cancel
  - Clear form

**Interaction Notes:**

- Form validation on blur/change
- Required fields highlighted if empty on submit
- Token image preview shows after upload
- Efficient tab navigation between fields
- Auto-save draft (optional, future enhancement)

**Design File Reference:** `character-form.figma` (to be created)

---

#### Character Management Panel

**Purpose:** View and manage all characters in campaign

**Key Elements:**

- Filter tabs: All / PCs / NPCs / Enemies
- Search bar (filter by name)
- Character list:
  - Each item shows:
    - Character name
    - Type badge (PC/NPC/Enemy)
    - Token image thumbnail
    - HP/AC summary
  - Click character → Opens character details/edit
- "Add Character" button (floating action or top button)
- Bulk actions (future): Select multiple, delete

**Interaction Notes:**

- List updates in real-time as characters are added/edited
- Click character → Opens character details panel or modal
- Hover shows quick actions (edit, delete)
- Responsive: Scrollable list, pagination if many characters

**Design File Reference:** `character-management.figma` (to be created)

---

**Rationale & Trade-offs:**

- **Campaign Dashboard:** Card-based layout provides visual hierarchy and quick scanning. Alternative: Table view (more compact but less visual).
- **Map Viewer:** Fixed toolbar ensures tools always accessible. Trade-off: Uses vertical space, but map area is still maximized.
- **Character Stats Panel:** Slides in from right to avoid covering map center where most action happens. Alternative: Bottom panel (less intrusive but harder to read during gameplay).
- **Character Entry Form:** Modal keeps focus on data entry. Alternative: Inline form (faster but more cluttered).

**Assumptions:**

- Desktop viewport: 1920x1080 minimum, optimized for 1366x768 and up
- Toolbar height: ~60px
- Sidebar width: 250px expanded, 60px collapsed
- Character stats panel: 300px width

**Questions for Validation:**

- Should character entry form be a modal or inline panel?
- Do we need a campaign overview screen, or go straight to map viewer?
- Should the map viewer have a minimap/overview window?

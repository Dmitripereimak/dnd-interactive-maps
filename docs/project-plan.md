# DnD Interactive Map Application - Project Plan

**Version:** 1.0  
**Date:** 2024-12-19  
**Status:** Planning Phase

---

## Executive Summary

This project is a web-based interactive map application for Dungeons & Dragons (DnD) game masters. The application allows DMs to upload custom map images, manage multiple campaigns, place and move character tokens on maps, and maintain campaign notes. The system will support fog of war, grid-based movement, and a comprehensive character/NPC management system.

**Key Value Proposition:** A single-user (initially) web application that provides DMs with a flexible, cloud-based tool for managing interactive battle maps and campaign data, accessible across devices.

---

## Project Goals

1. **Primary Goal:** Create a React-based web application for managing DnD interactive maps with character token placement and movement
2. **Data Persistence:** Implement cloud database storage for campaigns, maps, and characters to enable session continuity
3. **User Experience:** Provide an intuitive interface for map navigation, token management, and campaign organization
4. **Future Scalability:** Design architecture to support multi-user functionality in future phases

---

## Technical Architecture

### Technology Stack

- **Frontend Framework:** React
- **Database:** Cloud database (Firebase Firestore or similar)
- **Authentication:** Firebase Auth or similar cloud auth service
- **Image Storage:** Cloud storage service (Firebase Storage or similar)
- **State Management:** React Context API or Redux (TBD)
- **Map Rendering:** Canvas API or SVG-based solution
- **Responsive Design:** CSS Grid/Flexbox with mobile-first approach

### Architecture Decisions

1. **No Backend Server:** Application will be frontend-only with direct database connections
2. **Cloud-First:** All data stored in cloud database for cross-device access
3. **Progressive Web App (PWA):** Consider PWA capabilities for offline functionality (future)
4. **Component-Based:** Modular React components for reusability

### Database Structure

```
Users Collection
├── userId
│   ├── email
│   ├── displayName
│   └── campaigns[] (array of campaign IDs)

Campaigns Collection
├── campaignId
│   ├── userId (owner)
│   ├── name
│   ├── description
│   ├── createdAt
│   ├── updatedAt
│   ├── maps[] (array of map IDs)
│   ├── characters[] (array of character IDs)
│   └── notes[] (array of note objects)

Maps Collection
├── mapId
│   ├── campaignId
│   ├── name
│   ├── imageUrl (stored in cloud storage)
│   ├── imageFormat
│   ├── gridSize (pixels per grid square)
│   ├── gridEnabled (boolean)
│   ├── fogOfWarLayers[] (array of fog layer data)
│   ├── tokens[] (array of token placements)
│   ├── notes[] (array of map-specific notes)
│   └── createdAt

Characters Collection
├── characterId
│   ├── campaignId
│   ├── name
│   ├── type (character|npc|enemy)
│   ├── tokenImageUrl
│   ├── attributes
│   │   ├── hp
│   │   ├── ac
│   │   ├── stats (STR, DEX, CON, INT, WIS, CHA)
│   │   └── notes
│   └── createdAt

Token Placements (embedded in Maps)
├── tokenId
│   ├── characterId
│   ├── position (x, y coordinates)
│   ├── gridPosition (grid square coordinates)
│   └── visible (boolean for fog of war)
```

---

## Feature Requirements

### Phase 1: MVP Core Features

#### 1. User Authentication & Campaign Management

- **FR1:** User can register and login with email/password
- **FR2:** User can create multiple campaigns
- **FR3:** User can edit campaign name and description
- **FR4:** User can delete campaigns
- **FR5:** User can switch between campaigns

#### 2. Map Management

- **FR6:** User can upload map images (PNG, JPG, WebP formats)
- **FR7:** User can view list of maps in a campaign
- **FR8:** User can delete maps
- **FR9:** User can navigate between different maps
- **FR10:** User can view map in full-screen mode
- **FR11:** User can zoom in/out on maps (mouse wheel or controls)
- **FR12:** User can pan maps (click and drag)

#### 3. Grid System

- **FR13:** User can enable/disable grid overlay on maps
- **FR14:** Grid displays as square grid (configurable size)
- **FR15:** User can configure grid size (pixels per square)
- **FR16:** Tokens snap to grid when placed/moved (recommended: YES for better gameplay)

#### 4. Character/NPC/Enemy Management

- **FR17:** User can create characters with pre-defined library templates
- **FR18:** User can assign token images/icons to characters
- **FR19:** User can edit character attributes (HP, AC, stats, notes)
- **FR20:** User can delete characters
- **FR21:** User can view list of all characters/NPCs/enemies in campaign
- **FR22:** User can filter characters by type (character/NPC/enemy)

#### 5. Token Placement & Movement

- **FR23:** User can select character from list and place token on map
- **FR24:** User can move tokens by dragging with mouse
- **FR25:** Tokens maintain position when switching between maps
- **FR26:** User can remove tokens from map
- **FR27:** User can see which character a token represents (hover/click)

#### 6. Fog of War

- **FR28:** User can draw fog of war areas on map
- **FR29:** User can erase/reveal fog of war areas
- **FR30:** Fog of war persists per map
- **FR31:** Tokens can be hidden/revealed based on fog of war

#### 7. Notes & Journal

- **FR32:** User can add notes to individual maps
- **FR33:** User can add campaign-level notes
- **FR34:** User can edit and delete notes
- **FR35:** Notes are saved and persist across sessions

### Phase 2: Enhanced Features (Post-MVP)

- Multi-user support (share campaigns with other DMs)
- Map layers (multiple image layers)
- Hex grid support
- Token rotation
- Measurement tools (distance ruler)
- Initiative tracker integration
- Export/import campaigns
- Map templates library
- Advanced fog of war (per-player visibility)

### Non-Functional Requirements

- **NFR1:** Application must be responsive and work on mobile/tablet devices
- **NFR2:** Map images up to 10MB should load within 3 seconds
- **NFR3:** Token movement should be smooth (60fps) with up to 50 tokens on screen
- **NFR4:** All data must be automatically saved to cloud database
- **NFR5:** Application must handle offline scenarios gracefully (show connection status)
- **NFR6:** User authentication must be secure (encrypted passwords, secure tokens)

---

## User Interface Design Goals

### Overall UX Vision

The application should feel like a digital battle map with intuitive controls. The interface should be clean and uncluttered, allowing the map to be the focal point. Controls should be easily accessible but not intrusive.

### Core Screens and Views

1. **Login/Register Screen**

   - Simple email/password authentication
   - "Remember me" option

2. **Campaign Dashboard**

   - List of user's campaigns
   - Create new campaign button
   - Campaign cards showing name, description, last modified

3. **Campaign View**

   - Sidebar with:
     - Maps list
     - Characters/NPCs/Enemies list
     - Campaign notes
   - Main area: Map viewer or campaign overview

4. **Map Viewer**

   - Full-screen map display
   - Toolbar with:
     - Grid toggle
     - Fog of war tools
     - Zoom controls
     - Character selector dropdown
   - Token placement and movement area

5. **Character Management Panel**

   - List view of all characters
   - Create/Edit character modal/form
   - Character attributes editor

6. **Notes Panel**
   - Tabbed interface for map notes vs campaign notes
   - Rich text editor (or markdown)

### Target Platforms

- **Primary:** Web Responsive (Desktop, Tablet, Mobile)
- **Mobile Support:** Touch-optimized controls for token movement
- **Desktop:** Mouse and keyboard support

### Accessibility

- WCAG AA compliance (minimum)
- Keyboard navigation support
- Screen reader compatibility for text content

---

## Implementation Phases

### Phase 1: Foundation (Week 1-2)

1. Set up React project with build tools
2. Configure cloud database (Firebase or alternative)
3. Implement user authentication
4. Create basic routing structure
5. Set up project repository on GitHub

### Phase 2: Campaign Management (Week 2-3)

1. Campaign CRUD operations
2. Campaign dashboard UI
3. Campaign selection/switching

### Phase 3: Map Management (Week 3-4)

1. Image upload functionality
2. Cloud storage integration
3. Map list and navigation
4. Basic map viewer

### Phase 4: Map Interaction (Week 4-5)

1. Zoom and pan functionality
2. Grid system implementation
3. Grid configuration UI

### Phase 5: Character Management (Week 5-6)

1. Character CRUD operations
2. Character library/templates
3. Token image assignment
4. Character attributes management

### Phase 6: Token System (Week 6-7)

1. Token placement on maps
2. Token dragging/movement
3. Token persistence
4. Token-character association

### Phase 7: Fog of War (Week 7-8)

1. Fog of war drawing tools
2. Fog layer storage
3. Token visibility logic

### Phase 8: Notes System (Week 8)

1. Notes CRUD operations
2. Notes UI panel
3. Map vs Campaign notes

### Phase 9: Polish & Testing (Week 9-10)

1. Mobile responsiveness
2. Performance optimization
3. Bug fixes
4. User testing

---

## GitHub Repository Setup

### Repository Structure

```
dnd-interactive-maps/
├── .github/
│   └── workflows/          # CI/CD workflows (if needed)
├── public/
│   └── index.html
├── src/
│   ├── components/        # React components
│   ├── pages/            # Page components
│   ├── services/         # Database/auth services
│   ├── hooks/            # Custom React hooks
│   ├── utils/            # Utility functions
│   ├── styles/           # CSS/styling
│   └── App.js            # Main app component
├── docs/                 # Documentation
│   └── project-plan.md
├── .gitignore
├── package.json
├── README.md
└── LICENSE
```

### Initial Setup Steps

1. Create new repository on GitHub (public)
2. Initialize local git repository
3. Add remote origin
4. Create initial commit with project structure
5. Push to GitHub

---

## Recommendations

### Snap-to-Grid for Tokens

**Recommendation: YES, implement snap-to-grid**

**Rationale:**

- Provides cleaner, more organized token placement
- Easier to align tokens with map features
- Standard practice in DnD map tools
- Can be made optional (toggle) for flexibility

### Database Choice

**Recommendation: Firebase (Firestore + Storage + Auth)**

**Rationale:**

- No backend server required (matches requirement)
- Real-time updates capability
- Built-in authentication
- Cloud storage for images
- Generous free tier
- Easy React integration

**Alternative:** Supabase (PostgreSQL-based, similar features)

### State Management

**Recommendation: React Context API + useReducer**

**Rationale:**

- Built into React (no additional dependencies)
- Sufficient for MVP scope
- Can migrate to Redux if complexity grows
- Simpler learning curve

---

## Open Questions & Risks

### Open Questions

1. What is the maximum map image size we should support?
2. Should there be limits on number of campaigns/maps per user?
3. What character template library should we use? (DnD 5e standard?)
4. Should token movement have animation or be instant?
5. What is the target browser support? (IE11? Modern browsers only?)

### Key Risks

1. **Performance with large maps:** Large image files may cause performance issues

   - _Mitigation:_ Image optimization, lazy loading, canvas optimization

2. **Database costs:** Cloud database usage may exceed free tier

   - _Mitigation:_ Monitor usage, implement data limits, optimize queries

3. **Mobile usability:** Touch-based token movement may be challenging

   - _Mitigation:_ Extensive mobile testing, touch-optimized controls

4. **Image storage costs:** Many high-resolution maps could be expensive
   - _Mitigation:_ Image compression, storage limits per user

---

## Next Steps

1. **Immediate Actions:**

   - [ ] Set up GitHub repository
   - [ ] Initialize React project with create-react-app or Vite
   - [ ] Set up Firebase project (or chosen database)
   - [ ] Create initial project structure
   - [ ] Set up authentication service

2. **Development Start:**

   - Begin with Phase 1: Foundation
   - Implement authentication first
   - Then move to campaign management

3. **Documentation:**
   - Keep this plan updated as development progresses
   - Document API/service interfaces
   - Create component documentation

---

## Change Log

| Date       | Version | Description                  | Author      |
| ---------- | ------- | ---------------------------- | ----------- |
| 2024-12-19 | 1.0     | Initial project plan created | BMad Master |

---

**Document Status:** Ready for Development  
**Next Review:** After Phase 1 completion

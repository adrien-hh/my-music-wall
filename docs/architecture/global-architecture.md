# Global Architecture

This document defines the high-level architecture of MyMusicWall. It describes the responsibilities of each layer, the client-server interaction model, and the key architectural decisions. It serves as the foundation for all subsequent technical choices.


## 1. Overview
MyMusicWall follows a classic client-server architecture:
- A SPA frontend communicates with a REST API backend
- The backend handles all business logic and external integrations
- Data is persisted in a relational database


## 2. Frontend Responsibilities
- Render the album wall and UI components
- Handle user interactions (search, status changes, navigation)
- Call the backend REST API
- Hold no business state locally - the backend is the single source of truth

### Public vs. authenticated areas
- One public route: a landing page presenting the app and a sample of albums
- All other routes require authentication
- No server-side rendering - the app is a SPA, rendered entirely in the browser


## 3. Backend Responsibilities
- Expose a REST API (stateless, JSON)
- Handle all business logic (collection management, album statuses)
- Authenticate users and enforce data ownership (each user accesses only their own library)
- Proxy requests to the external music API
- Persist data to the database


## 4. Client-Server Interaction Model
- The frontend communicates exclusively with the backend via REST API calls
- All requests are stateless - each request includes an authentication token
- The authentication mechanism (provider and token format) will be defined at stack selection


## 5. Data Flow

### 5.1 Search and add an album
1. User searches for an album
2. Frontend calls backend search endpoint
3. Backend calls the external music API and returns normalized results
4. User selects an album
5. Backend persists the album with default status `TO_LISTEN` (or a directly chosen status)

### 5.2 Set album status
- User can set any status at any time: `TO_LISTEN`, `LIKED`, or `DISLIKED`
- Setting `LIKED` or `DISLIKED` on an unsaved album also adds it to the collection
- Backend updates the record on each status change

### 5.3 Browse the collection
1. Frontend requests the user's collection from the backend
2. Backend returns albums with their current statuses
3. Frontend renders the album wall
4. Filters (e.g. listened, liked) are applied via query parameters - no separate API calls

### 5.4 View album details
1. Frontend requests album details from the backend
2. Backend returns data from the local database
3. The external music API is not called again for albums already in the collection


## 6. External Dependencies
| Dependency | Role | Notes |
|---|---|---|
| Music API | Album search, metadata, cover art | Provider TBD (Spotify, Discogs, MusicBrainz) |
| Auth provider | User authentication | Mechanism TBD at stack selection |
| Relational database | Data persistence | Albums, statuses, user accounts, ... |


## 7. Key Architectural Decisions
- **Backend proxies the music API** - the frontend never calls the music API directly. This keeps credentials server-side and allows normalizing or switching providers without frontend impact.
- **Album metadata persistence** - whether metadata should be stored locally on first add or fetched on demand is not yet decided. See Open Questions.
- **Stateless REST API** - no server-side session. Authentication is token-based (details TBD).
- **SPA with a single public route** - the app is fully client-rendered. One public landing page exists to welcome unauthenticated users; all other routes require a valid session.


## 8. Open Questions
- Which music API will be used? (Spotify, Discogs, or MusicBrainz)
- Which authentication mechanism will be used? (in-house JWT, Keycloak, Auth0, etc.)
- What album metadata should be persisted locally, and what should remain fetched on demand?
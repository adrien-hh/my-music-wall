# Domain Entities

This document defines the core domain entities of MyMusicWall for the MVP scope.  
It is technology-agnostic and serves as the foundation for the data model and backend design.


## Entities Overview

| Entity | Responsibility |
|---|---|
| `User` | Represents an authenticated user with their own collection |
| `Album` | Represents an album sourced from an external music API |
| `CollectionEntry` | Represents the relationship between a user and an album, including its status |


## 1. User
Represents a registered user of the application.  
Each user has their own independent collection.

### Attributes
| Attribute | Type | Description |
|---|---|---|
| `id` | UUID | Unique identifier |
| `email` | String | Used for authentication |
| `password` | String | Hashed password |
| `username` | String | Display name |
| `createdAt` | DateTime | Account creation date |


## 2. Album
Represents an album retrieved from an external music API and stored locally.  
Album data is sourced externally (cover, title, artist, release year).  
It is not user-specific: the same album can appear in multiple users' collections.

### Attributes
| Attribute | Type | Description |
|---|---|---|
| `id` | UUID | Unique internal identifier |
| `externalId` | String | ID from the external music API |
| `title` | String | Album title |
| `artist` | String | Artist name (used for display and filtering) |
| `releaseYear` | Integer | Year of release |
| `coverUrl` | String | URL of the album cover image |


## 3. CollectionEntry
Represents the fact that a user has added an album to their collection.  
Carries the status assigned by the user.  
This is the central entity of the domain: it links a `User` to an `Album`.

### Attributes
| Attribute | Type | Description |
|---|---|---|
| `id` | UUID | Unique identifier |
| `userId` | UUID | Reference to the owning `User` |
| `albumId` | UUID | Reference to the `Album` |
| `status` | Enum | `TO_LISTEN`, `LIKED`, or `DISLIKED` |
| `addedAt` | DateTime | Date the album was added to the collection |

### Constraints
- A user cannot have the same album twice in their collection.
- Status is always set at creation (no statusless entry).


## Relationships
```
User ────< CollectionEntry >──── Album
```

- One `User` has many `CollectionEntry` items.
- One `Album` can appear in many `CollectionEntry` items.
- `CollectionEntry` is the join entity between `User` and `Album`.


## Status Values
| Value | Meaning |
|---|---|
| `TO_LISTEN` | Added to the collection, not yet listened |
| `LIKED` | Listened and liked |
| `DISLIKED` | Listened, not liked |

> "Listened" is a derived filter (`LIKED` + `DISLIKED`), not a status.
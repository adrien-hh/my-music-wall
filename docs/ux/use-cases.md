# Use Cases

This document defines the core use cases of MyMusicWall. Use cases describe the key interactions between users and the application, and serve as a foundation for functional design, data modeling, and implementation decisions.

## UC-01 — Browse the album wall
**Actor:** Any authenticated user  
**Goal:** Visually browse their album collection  
**Outcome:** The user sees a grid of album covers and can scroll through their library

## UC-02 — Search and add an album
**Actor:** Any authenticated user  
**Goal:** Find an album and add it to their collection  
**Outcome:** The album appears in their wall

## UC-03 — Set album status
**Actor:** Any authenticated user  
**Goal:** Mark an album with a status (To Listen, Liked, Disliked)  
**Outcome:** The album status is updated in the collection

## UC-04 — View album details
**Actor:** Any authenticated user  
**Goal:** See basic information about an album  
**Outcome:** A modal displays cover, title, artist, and available actions

## UC-05 — Sign up
**Actor:** New visitor  
**Goal:** Create an account  
**Outcome:** The user has a personal library and can start adding albums

## UC-06 — Log in / Log out
**Actor:** Registered user  
**Goal:** Access or leave their personal space  
**Outcome:** The user is authenticated and redirected to their wall, or logged out

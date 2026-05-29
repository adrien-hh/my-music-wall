# Technical Stack

This document defines the technical stack selected for MyMusicWall. It covers each layer of the application, from frontend to infrastructure, and documents the rationale behind each choice.

## 1. Frontend
- **Framework/library:** React / TypeScript
- **Styling:** Tailwind CSS

React is the reference library for component-based UIs.
TypeScript adds type safety and improves maintainability.
Tailwind enables fast iteration on a visual, component-heavy interface.


## 2. Backend
- **Framework:** Java / Spring Boot
- **API style:** REST

Spring Boot is a proven choice for REST APIs. Well-suited for structured, layered backend development.


## 3. Database
- **Engine:** PostgreSQL
- **Access:** Spring Data JPA / Hibernate

PostgreSQL is reliable, widely supported, and fits the relational data model.


## 4. Authentication
- **Solution:** Spring Security + JWT

No external dependency or hosted service.
Full control over the auth flow.


## 5. Music API
- **Provider:** Discogs API

Single API for metadata and cover art.
Simple token-based auth (no OAuth flow).
Generous rate limits (60 req/min), sufficient for personal use.
Free, no account restrictions on basic usage.


## 6. Infrastructure
- **Containerization:** Docker
- **Cloud:** Azure (AKS, ACR)
- **Provisioning:** Terraform

AKS for Kubernetes orchestration, ACR for image registry.
Terraform for infrastructure as code from day one.


## 7. CI/CD
- **Platform:** GitHub Actions

Native GitHub integration.
Covers build, test, image push to ACR, and deploy to AKS.


## 8. Dependencies / Notes
- Based on global architecture (`#3`)
- Music API choice may be revisited if Discogs coverage proves insufficient
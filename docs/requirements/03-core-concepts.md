# Phase 3 — Core Concepts & Actors

> Purpose: Identify the fundamental actors and data entities in the system before designing interfaces or architecture.

## 1. System Actors

*(Note: Actors are external participants who interact with the system, not database records.)*

- **The Administrator (Curator):** The owner of the system. Responsible for manually sourcing, reviewing, approving, and adding 100% of the gallery's content to ensure that only intended 2D/anime content is published.
- **The Public Visitor:** An anonymous user who browses the public-facing gallery to view and download images.

---

## 2. Core Domain Entities

### The Image (Wallpaper)
The primary content entity in the system. Each Image represents a wallpaper available for users to view and download.
* **Image Content:** The visual media source (file or URL) made available for the gallery.
* **Original Filename:** The original file name established by the Administrator for the image.
* **Description (Alt-Text):** A brief textual description associated with the image. 
- **Added Date:** The point in time when the image was added to the gallery.
* *(Future Evolution: Categories or tags may be introduced in later versions when the image volume necessitates filtering).*

---

## 3. High-Level Relationships

- The **Administrator** curates and publishes the **Images**.
- The **Public Visitor** views the grid of **Images**.
- The **Public Visitor** selects and downloads an individual **Image**.

--- 

## 4. Core Business Invariants

*(Note: The following rules must always be true, regardless of how the software is eventually engineered.)*

- **Strictly Read-Only Public Access:** Public users cannot modify gallery content, upload files, or submit data to the system.
- **Zero User State:** The system will never authenticate visitors or store user-specific data (no accounts, no favorites, no "likes").
- **Curator Authority:** Only the Administrator can modify the state of the gallery (adding or removing Images).

## Phase Completion Rule

This phase is complete when:

- System actors are explicitly separated from data entities.
- The core data structure is defined.
- Business invariants correctly reflect the project's constraints (e.g., no authentication and no user-specific state).
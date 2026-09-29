# 01 — Feature Decomposition

## 1. Purpose

This document breaks Visiverse Version 1 into its meaningful capabilities, shows who owns which behavior, and records the relationships, cross-cutting work, and open unknowns that the next document (Development Phases) needs in order to sequence the work.

It does not define implementation, architecture, technology, database or API design, or development order.

**Project size:** small-to-medium. Few features, but concentrated complexity in live updates, browsing stability, and position restoration.

---

## 2. Capability Decomposition

```text
Visiverse V1
│
├── Gallery Browsing
├── Detail View
├── Image Download
│   ├── Core download behavior
│   └── Entry points (gallery, detail view)
└── Live Gallery Management
    ├── Content Publication
    └── Live Propagation
```

Each capability owns its own reaction to live content changes. Live Gallery Management owns publication changes and their propagation to active visitors.

---

## 3. Capabilities

### 3.1 Gallery Browsing

Presents published images to the visitor and keeps browsing continuous.

* Initial gallery load
* Newest-first fixed ordering
* Batch loading and continuous scrolling
* Stop at end of gallery (no indicator)
* Empty-gallery state
* Initial-load failure with retry
* Batch-load failure with retry
* Individual image load failure (image hidden)
* **Reaction to live changes:** new images do not shift the viewport; removed images show the "removed" placeholder for the session

### 3.2 Detail View

Lets the visitor inspect one image at high resolution and move through the sequence.

* Open from a gallery image
* High-resolution display
* Next/previous navigation and boundary handling
* Detail-image load failure with retry
* Return to the gallery position on close
* **Reaction to live changes:** the navigation sequence is not disturbed by additions; a removed image stays visible until the visitor leaves

### 3.3 Image Download

One capability with two parts, so that entry points do not duplicate behavior.

* **Core download behavior:** progress indication, original filename preserved, success completion, failure message with retry, no partial or corrupted file, rejection of removed images
* **Entry points:** quick download from the gallery, download from the detail view

### 3.4 Live Gallery Management

Manages publication changes and their propagation to active visitors. The external media source is a dependency, not a Visiverse capability.

* **Content Publication:** administrator adds and removes published images through the external source without a website redeploy (mostly setup and process, little visitor-facing build work)
* **Live Propagation:** delivering additions and removals to active visitors within the target latency, without a manual refresh

This is the main dependency hub. The three visitor-facing capabilities above each depend on it for their live-change behavior.

---

## 4. Shared Behaviors

These cross capability boundaries and are not separate features.

| Behavior              | Shared between                                         | Note                                                                                                                  |
| --------------------- | ------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------- |
| Active image sequence | Gallery Browsing, Detail View                          | Gallery establishes the sequence; Detail View navigates it.                                                           |
| Browsing continuity   | Gallery Browsing, Detail View, Live Gallery Management | Preserving and restoring position, and staying stable while content changes. Highest interaction risk in the project. |

---

## 5. Work Dependencies and Interactions

One table covers both. "Hard" means the work cannot be completed without it; "interacts" means the two must be reconciled but neither strictly blocks the other.

| Work                                    | Depends on / interacts with                   | Type      | Why                                                                |
| --------------------------------------- | --------------------------------------------- | --------- | ------------------------------------------------------------------ |
| Gallery Browsing                        | External media source                         | Hard      | Published content originates there.                                |
| Detail View                             | Gallery Browsing                              | Hard      | Entry point and source of the active sequence.                     |
| Core download behavior                  | External media source                         | Hard      | Source of the file. Independent of gallery and detail UI.          |
| Quick-download entry point              | Gallery Browsing, core download               | Hard      | Sits on the gallery and triggers core download.                    |
| Detail-download entry point             | Detail View, core download                    | Hard      | Sits on the detail view and triggers core download.                |
| Content Publication                     | External media source                         | Hard      | Content changes are made there.                                    |
| Live Propagation                        | Content Publication                           | Hard      | Nothing to propagate without publication changes.                  |
| Live-change reactions (each capability) | Live Propagation, the owning capability       | Hard      | Reactions need both the change signal and the surface that reacts. |
| Browsing continuity                     | Gallery Browsing, Detail View                 | Hard      | Needs both surfaces to exist.                                      |
| Browsing continuity                     | Live Propagation                              | Interacts | Position restoration must stay correct while content changes.      |
| Removed-image handling                  | Detail View, Image Download, Gallery Browsing | Interacts | One removal must produce consistent behavior on three surfaces.    |

---

## 6. Cross-Cutting Work

Not features, but each produces work that must land somewhere in the plan. They apply across the capabilities above.

* Responsive behavior for desktop and mobile browsers
* Browser compatibility (Chromium, Safari, Firefox)
* Keyboard operation and baseline accessibility semantics
* Performance targets and their validation (before launch and real-user afterward)
* Search-engine discoverability
* Abuse-traffic protection
* Zero tracking and no persistent user data
* Secure transport, network resilience
* $0/month operating cost

Where these touch the capability work, they are verified per capability, not as a separate phase at the end.

---

## 7. Non-Feature Work

Real V1 work that is not a product capability and needs a place in the plan.

* Preparing the baseline curated image collection (a stated launch success criterion)
* First production launch and post-launch real-user measurement

---

## 8. Decomposition-Relevant Unknowns

Unknowns are recorded here, not decided.

| Unknown                                                                                                                                                     | Affects                                           | Status                |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- | --------------------- |
| Which position does the return from the detail view restore: where the visitor opened it, or the last image they viewed? The requirements documents differ. | Browsing continuity, Detail View                  | Needs a decision      |
| Does detail-view navigation load further batches when it reaches the end of the loaded set, and what counts as the "last" image?                            | Boundary between Gallery Browsing and Detail View | Needs a decision      |
| What happens if the visitor navigates onto an image removed after the sequence was established?                                                             | Detail View, live-change reactions                | Needs a decision      |
| Filename collision handling                                                                                                                                 | Image Download (filename preservation)            | Open, deferred        |
| Content licensing and sourcing policy                                                                                                                       | Baseline content preparation                      | Open, deferred        |
| Can the live-update latency be met within $0 and 100 concurrent visitors?                                                                                   | Live Propagation, sequencing                      | Open feasibility risk |
| Can high-quality media coexist with the performance targets?                                                                                                | Detail View, Gallery Browsing, sequencing         | Open feasibility risk |

The two feasibility risks should get an early checkpoint in Development Phases, not a late one.

---

## 9. Capability to Requirement Mapping

Maps to the source requirements (Phase 5 sections, Phase 9 acceptance criteria) so coverage can be checked.

| Capability              | Phase 5 section             | Acceptance criteria                                                   |
| ----------------------- | --------------------------- | --------------------------------------------------------------------- |
| Gallery Browsing        | Grid View, Edge Cases       | AC-01 to AC-07, AC-18 to AC-20                                        |
| Detail View             | Detail View                 | AC-08 to AC-11                                                        |
| Image Download          | Download                    | AC-12 to AC-17, AC-44                                                 |
| Live Gallery Management | Administrator Workflow      | AC-21, AC-24, AC-37                                                   |
| Live-change reactions   | Grid, Detail, Administrator | AC-22 and AC-24 (gallery), AC-23 and AC-25 (detail), AC-17 (download) |
| Cross-cutting           | Non-functional requirements | AC-26 to AC-36, AC-38 to AC-43, AC-45, AC-48                          |
| Non-feature work        | Success criteria (Phase 1)  | AC-38 (measurement)                                                   |

Abuse-traffic protection is a stated non-functional requirement with no acceptance criterion. Noted here so it is not lost.

---

## 10. Boundary

```text
Requirements Analysis → Feature Decomposition → Development Phases → Design / Architecture
```

---

## 11. Out of Scope for This Document

* Framework and technology choices
* API and database design
* Component structure
* Infrastructure
* Caching
* External-service configuration
* Implementation algorithms
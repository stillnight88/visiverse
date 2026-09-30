# 02 — Development Phases

**Status:** Final

## 1. Purpose

This document sequences Visiverse V1 into development phases, based on the final Feature Decomposition. It answers: **in what coherent increments should V1 be built, and why in that order?**

It does not define technology choices, API or database design, component structure, infrastructure, implementation algorithms, individual coding tasks, or Git/branch structure. Phases are planning boundaries; a phase may later become several issues and branches.

---

## 2. Sequencing Strategy

```text
1. Media/Read Foundation & Gallery
        ↓
2. Live Gallery Management & Gallery Reactions
        ↓
3. Image Download
        ↓
4. Detail View & Browsing Continuity
        ↓
5. Integrated Validation & Hardening
        ↓
6. Launch & Real-User Measurement

Alongside phases 2 to 5: Content Preparation (non-development workstream)
```

### Why this order

* **Hub first.** Live Gallery Management is the dependency hub. Building the live mechanism right after Gallery means Download and Detail View are built live-aware, not retrofitted.
* **Early feasibility.** The two feasibility risks are checked in Phase 1 and confirmed in Phase 2, before later work is built around them.
* **Download before Detail View is a sequencing choice, not a dependency.** Detail View depends only on Gallery. Download comes first because "browse → download" is the smallest useful increment for a solo project.
* **Solo project.** There is no real parallel development. Content Preparation is the only workstream that runs alongside the phases.

### Ownership rules

* Each behavior is built in the phase of its owning capability, once and only once.
* Live-change reactions are built together with the surface that reacts, and only after the live mechanism exists.
* Cross-cutting requirements are attached to the phase where they first matter, then revalidated later.

---

## 3. Phase Overview

| Phase | Name                                        | Purpose                                                                         | Effort |
| ----- | ------------------------------------------- | ------------------------------------------------------------------------------- | ------ |
| 1     | Media/Read Foundation & Gallery             | Establish the content read path and build the primary gallery                   | Large  |
| 2     | Live Gallery Management & Gallery Reactions | Build the live-change mechanism and the gallery's reaction to it                | Medium |
| 3     | Image Download                              | Build download behavior, including the removed-image rule                       | Small  |
| 4     | Detail View & Browsing Continuity           | Build high-resolution viewing, navigation, and position restoration, live-aware | Large  |
| 5     | Integrated Validation & Hardening           | Validate interactions and cross-cutting requirements together                   | Medium |
| 6     | Launch & Real-User Measurement              | Launch with final content and begin real-user measurement                       | Small  |
| —     | Content Preparation (alongside)             | Prepare the curated launch collection                                           | Medium |

Effort is relative sizing, not a duration estimate. See section 8.

---

## 4. Phases

### Phase 1 — Media/Read Foundation & Gallery

**Builds**

* Media foundation: external media source, a small development collection, confirmation that added date and original filename are available, and confirmation that development content can be added or removed through the external source without a website redeploy.
* Gallery Browsing: initial load, newest-first ordering, batch loading and continuous scrolling, end-of-gallery behavior, empty state, initial-load failure and batch-load failure with retry, individual image failure.

**Checks (not full implementation)**

* **Media/performance checkpoint:** can representative media coexist with the performance targets? Cover both gallery thumbnails and the high-resolution media that Detail View will use.
* **Propagation spike:** a time-boxed check of whether live updates can plausibly meet the latency target within $0 and about 100 concurrent visitors. It is a feasibility check, not the live mechanism.

**Cross-cutting first matters here:** responsive behavior, browser compatibility, keyboard and accessibility basics for the gallery, initial performance, HTTPS, zero tracking, crawler access to gallery content.

**Depends on:** external media source availability.

**Done when**

* gallery shows the published sequence correctly, with batch, empty, and failure behavior as specified
* one failed image does not break the gallery
* media/performance checkpoint result is recorded
* propagation spike result is recorded and feeds Phase 2
* cross-cutting checks above pass for the gallery

---

### Phase 2 — Live Gallery Management & Gallery Reactions

**Builds**

* Content Publication: operational workflow for adding and removing published images without a website redeploy.
* Live Propagation: additions and removals reach active visitors without a manual refresh, within the latency target.
* Gallery reactions: new images appear at the top without shifting the visitor's position; removed images become the placeholder for the session and disappear after refresh.

**Gate:** confirm propagation feasibility against $0/month and about 100 concurrent visitors. If the target cannot be met within the current constraints, the resulting requirement/constraint conflict must be explicitly resolved before Phase 3.

**Cross-cutting first matters here:** abuse-traffic protection (the live mechanism is the main source of repeated requests; this placement is a judgment and can be revisited), live-update latency, concurrent capacity, network resilience of the live mechanism.

**Depends on:** Phase 1.

**Done when**

* additions and removals propagate without refresh within the target
* live additions do not disturb an actively browsing visitor
* removed images follow the placeholder behavior
* propagation feasibility is confirmed, or the resulting requirement/constraint conflict is explicitly raised and resolved

---

### Phase 3 — Image Download

**Builds**

* Core download behavior: progress indication, original filename preserved, success, failure message with retry, no corrupted or partial file, **rejection of removed images (owned here)**.
* Gallery entry point: quick download.

**Gate:** filename collision handling is an open engineering decision. The original-filename requirement stays authoritative. Resolve before this phase completes, and before final content is named.

**Depends on:** Phase 1 (source of files). Removed-image rejection is verified using an image removed through the Phase 2 mechanism.

**Done when**

* gallery downloads work with the required filename
* failed downloads never produce a corrupted or partial file
* removed-image download fails per the defined behavior

---

### Phase 4 — Detail View & Browsing Continuity

**Builds**

* Detail View: open from the gallery, high-resolution display, next and previous navigation, boundary behavior, detail-image failure with retry.
* Detail download entry point.
* Active image sequence and browsing continuity: return to the required gallery position.
* Live reactions, built in: additions do not alter the active sequence; removal of the viewed image does not close or interrupt the view.

**Gates (all resolved before this phase starts)**

* Contextual return: the requirements differ on whether return restores the position where the view was opened or the last viewed image.
* Detail navigation and batches: whether navigation triggers loading further batches, and what counts as the "last" image. This affects the Gallery/Detail boundary, so the Gallery built in Phases 1 and 2 must not preclude either outcome.
* Navigating onto an image removed after the sequence was established.

**Also checked here:** performance of high-resolution detail images, keyboard and accessibility for detail navigation and download controls.

**Depends on:**

* **Gallery — hard dependency**
* **Live propagation — required capability already established in Phase 2**
* **Core download — sequencing dependency for the Detail download entry point**

**Done when**

* detail view, navigation, boundaries, and failure recovery work
* detail download works
* live additions and removals behave as defined during detail viewing
* browsing continuity holds, including while content changes
* the three gates above were resolved and implemented

---

### Phase 5 — Integrated Validation & Hardening

**Purpose:** the final integrated layer, not the first time cross-cutting requirements are considered. Runs on near-final content.

**Validates**

* Interactions: Gallery + Detail View, Gallery + Download, Detail View + Download, Live Propagation + each surface, Download + removed images, Browsing continuity + live changes.
* Final targets: LCP, INP, CLS, scroll responsiveness, concurrent capacity.
* Desktop and mobile behavior, Chromium, Safari, Firefox.
* Keyboard operation and baseline assistive semantics.
* Network resilience, SEO discoverability, privacy and zero tracking, HTTPS, abuse protection.

**Depends on:** Phases 1 to 4, and a sufficiently mature content collection.

**Done when**

* cross-cutting and integrated behavior is validated
* known high-risk interactions have been exercised
* remaining failures are resolved or explicitly recorded as launch blockers or accepted limitations

---

### Phase 6 — Launch & Real-User Measurement

**Work**

* Finalize and lock the curated collection prepared earlier.
* Run the defined acceptance criteria against the production candidate and confirm $0/month operation.
* Deploy and verify gallery, Detail View, downloads, and live behavior in production.
* Begin real-user measurement against the percentile targets and record deviations.

**Done when**

* final content is in place, acceptance checks pass, production is verified, V1 is public, and real-user measurement is active.

---

## 5. Content Preparation (alongside Phases 2 to 5)

Preparing the curated launch collection is real V1 work but not development, so it runs alongside the phases.

* **Content sourcing and collection can begin early** once the Phase 1 media checkpoint provides sufficient evidence about the media characteristics the project can support.
* **Final curation and naming depend on the sourcing/licensing decision and filename collision handling.**
* **Near-final by the start of Phase 5**, so integrated validation runs on the real collection. Phase 6 must not be the first test of the launch content.

---

## 6. Cross-Cutting Placement

| Concern                    | First matters / checked                  | Revalidated    |
| -------------------------- | ---------------------------------------- | -------------- |
| Responsive and browser     | Phase 1                                  | Phases 4, 5    |
| Keyboard and accessibility | Phase 1                                  | Phases 4, 5    |
| Performance                | Phase 1 (thumbnails), Phase 4 (high-res) | Phases 2, 5, 6 |
| SEO                        | Phase 1                                  | Phase 5        |
| Privacy and zero tracking  | Phase 1                                  | Phase 5        |
| HTTPS                      | Phase 1                                  | Phases 5, 6    |
| Network resilience         | Phase 1                                  | Phase 5        |
| Live-update latency        | Phase 2                                  | Phase 5        |
| 100 concurrent visitors    | Phase 2                                  | Phase 5        |
| Abuse protection           | Phase 2                                  | Phase 5        |
| $0/month                   | Phase 1 (spike)                          | Phase 6        |
| Real-user measurement      | Phase 6                                  | —              |

Each phase's "Done when" includes the cross-cutting checks that first matter in it.

---

## 7. Decision Gates

| Decision                                                     | Needed before                          | Notes                                                                                                  |
| ------------------------------------------------------------ | -------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Media/performance feasibility                                | End of Phase 1                         | Checkpoint result recorded                                                                             |
| Propagation feasibility                                      | Spike in Phase 1, confirmed in Phase 2 | Conflict must be raised and explicitly resolved if the target cannot be met within current constraints |
| Contextual return position                                   | Phase 4 starts                         | Requirements differ                                                                                    |
| Detail navigation and batch loading, meaning of "last"       | Phase 4 starts                         | Gallery must not preclude either outcome                                                               |
| Navigating onto a removed image                              | Phase 4 starts                         | Must be consistent with existing removal behavior                                                      |
| Filename collision handling                                  | Phase 3 completes                      | Also affects naming of final content                                                                   |
| Content sourcing and licensing, and whether it blocks launch | Phase 5 starts                         | Needs an explicit decision, since content preparation starts earlier                                   |

The plan does not convert these unknowns into requirements.

---

## 8. Effort and Timeline

Effort is relative (Small, Medium, Large), because durations for a part-time solo project would be false precision. The one-month target remains a flexible target.

Primary schedule risks: browsing continuity, Detail View sequence behavior, live propagation feasibility, media performance, and content preparation with an unresolved licensing decision.

If the work exceeds the target, identify the source of the overrun first, and do not compress validation or drop requirements.

---

## 9. Capability-to-Phase Coverage

| Capability / Work                 | Phase                                               |
| --------------------------------- | --------------------------------------------------- |
| Gallery Browsing                  | 1 (reactions in 2)                                  |
| Content Publication               | 1 (development read path), 2 (operational workflow) |
| Live Propagation                  | 2                                                   |
| Core download                     | 3                                                   |
| Gallery download entry point      | 3                                                   |
| Detail View, sequence, continuity | 4                                                   |
| Detail download entry point       | 4                                                   |
| Live reactions: gallery / detail  | 2 / 4                                               |
| Removed-image download rule       | 3                                                   |
| Cross-cutting work                | Section 6                                           |
| Content preparation               | Alongside 2 to 5                                    |
| Launch and measurement            | 6                                                   |

---

## 10. Boundary

```text
Requirements Analysis → Feature Decomposition → Development Phases → Design / Architecture
```

This document defines when and why major work is done, not how the system is built.

# Phase 8 — Risks & Unknowns

> **Purpose:** Identify uncertainties and threats that could prevent Visiverse from achieving its defined objectives and constraints, while distinguishing accepted risks from unresolved unknowns.

---

## 1. Business & Operational Risks

### Risk 1: Traffic & $0 Budget

* **Threat:** Unexpected growth or automated traffic could increase resource consumption beyond the limits of the services available under the $0/month Version 1 constraint.
* **Impact:** The gallery could experience degraded performance, reduced availability, or service restrictions.
* **Response:** The $0/month constraint applies to the initial Version 1 launch. If the gallery reaches a level of usage that cannot reasonably be supported within this constraint, the Administrator accepts that additional operational spending may become necessary.

### Risk 2: External Media Service Dependency

* **Threat:** The gallery depends on an external media service for the availability and delivery of published images. Service outages, restrictions, or changes to its usage policies could affect the gallery.
* **Impact:** Images may become unavailable, performance may degrade, or the $0/month operational constraint may no longer be sustainable.
* **Status:** Accepted risk for Version 1.

### Risk 3: Free-Tier Policy Changes

* **Threat:** An external service provider may change its free-tier limits, pricing, or usage policies after the system has been deployed.
* **Impact:** The project may no longer be able to operate within the defined $0/month constraint.
* **Response:** The project accepts this as an external risk. If the existing service arrangement becomes unsuitable, the Administrator may evaluate alternatives or accept additional operational costs.

### Risk 4: Content Rights & Licensing

* **Threat:** Images sourced from third parties may not have sufficient rights or permission for redistribution as downloadable wallpapers.
* **Impact:** Individual images or the gallery itself could become subject to removal requests or other legal/operational consequences.
* **Status:** **Unresolved.**
* **Decision:** The project will not define a specific V1 content-licensing policy at this stage. The Administrator's acceptable sourcing criteria remain an open question.

### Risk 5: Organic Discovery May Not Produce Expected Traffic

* **Threat:** Making the gallery discoverable and indexable by search engines does not guarantee meaningful organic traffic.
* **Impact:** Visiverse may receive fewer visitors than expected despite satisfying its defined SEO requirements.
* **Status:** Accepted business uncertainty for Version 1.

---

## 2. Performance & User Experience Risks

### Risk 6: Media Quality vs. Performance

* **Threat:** High-quality, high-resolution images may require significant resources to load and could make the defined frontend performance targets more difficult to achieve, particularly on mobile networks.
* **Impact:** The gallery may fail to meet its defined LCP, INP, CLS, or browsing-responsiveness targets.
* **Status:** Open risk.
* **Response:** The project must balance the requirement for high-quality images with the defined performance requirements during later engineering and validation.

### Risk 7: Live Updates vs. Operational Constraints

* **Threat:** Providing live gallery updates to active visitors within the defined 5-second propagation target may place additional demands on the services supporting the gallery.
* **Impact:** The live-update requirement may conflict with the $0/month operational constraint or the expected capacity of 100 concurrent active visitors.
* **Status:** Open risk.
* **Response:** The engineering stage must determine whether the defined live-update requirement can be satisfied within the established operational constraints.

---

## 3. Functional & Behavioral Risks

### Risk 8: Continuous-Scroll Batch Failure

* **Threat:** A temporary failure while loading a subsequent image batch could prevent additional gallery content from becoming available.
* **Impact:** The visitor could be unable to continue browsing beyond the currently loaded content.
* **Response:** If loading a subsequent batch fails, the system must provide a retry action that allows the visitor to retry loading that batch.

### Risk 9: Contextual Return Complexity

* **Threat:** Preserving the visitor's exact gallery position after leaving the detail view may become difficult when the gallery changes during the visitor's browsing session.
* **Impact:** The visitor could return to an incorrect position and lose their previous browsing context.
* **Status:** Open risk.
* **Response:** The requirement to restore the visitor's exact position remains protected. The engineering stage must determine how this requirement can be reliably satisfied.

### Risk 10: Live Gallery Changes During Active Browsing

* **Threat:** Images may be added or removed while a visitor is actively browsing or viewing an image in detail.
* **Impact:** Live changes could unexpectedly disrupt the visitor's current browsing position or detail-view navigation.
* **Response:** The behaviors defined in the Functional Requirements remain authoritative: live additions must not unexpectedly shift the visitor's current position or alter the active detail-view sequence, while removed images must follow the defined removal behavior.

### Risk 11: Filename Collision

* **Threat:** Different images may have identical original filenames while the system requires the original filename to be preserved during downloads.
* **Impact:** Identical filenames could create ambiguity or prevent the system from reliably maintaining the intended relationship between an image and its original filename.
* **Status:** **Unresolved.**
* **Decision:** The handling of filename collisions will be determined during the later engineering/design stage.

---

## 4. Project Timeline Risk

### Risk 12: One-Month Timeline Squeeze

* **Threat:** The defined V1 scope may require more development effort than initially expected, particularly for performance-sensitive and interactive behaviors.
* **Impact:** Initial deployment may take longer than the targeted one-month development period.
* **Response:** The one-month period is treated as a **target rather than a hard constraint**. If additional time is required to meet the accepted V1 requirements, the project may extend beyond the target.

---

## 5. Open Risks & Unknowns

The following uncertainties remain intentionally unresolved and should not be converted into requirements until sufficient information is available:

* **Content Rights & Licensing:** Acceptable sourcing and redistribution criteria for images.
* **Filename Collision:** How identical filenames across different images should be handled.
* **Live Update Feasibility:** Whether the 5-second propagation requirement can be maintained within the $0/month and expected-capacity constraints.
* **Performance Feasibility:** Whether the high-quality media requirement can consistently coexist with the defined performance targets.

These items are recorded so that they remain visible during later engineering and validation rather than becoming accidental assumptions.

---

## Phase Completion Rule

This phase is complete when:

* Known threats to the project's objectives and constraints are explicitly documented.
* Important risks have an identified response, acceptance status, or unresolved status.
* Risks are separated from confirmed requirements and business decisions.
* Technology and architecture are not prematurely selected as risk mitigations.
* Open uncertainties are clearly identified for later investigation and decision-making.

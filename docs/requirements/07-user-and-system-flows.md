# Phase 7 — User & System Flows

> **Purpose:** Translate the product scope and functional requirements into clear user journeys and system interactions before implementation.

---

## Flow 1: Initial System Load

*Describes the visitor's first interaction after navigating to the gallery.*

1. **Visitor** navigates to the gallery.
2. **System** requests the first batch of published images, containing no more than 20 images.
3. **System Evaluation:**

   * **If images are available:** The system displays the images in the fixed descending chronological order (newest added first).
   * **If the gallery contains no published images:** The system displays the exact message: *"No images available at this time."*
   * **If the gallery data cannot be loaded:** The system displays an error state with the message: *"Unable to load images. Please try again."*
4. **Visitor** may retry the failed gallery load when the error state is displayed.

---

## Flow 2: Continuous Browsing

*Describes how additional gallery content becomes available as the visitor browses.*

1. **Visitor** scrolls toward the end of the currently loaded batch.
2. **System** loads the next batch of no more than 20 images before the visitor reaches the end of the current batch.
3. **System Evaluation:**

   * **If additional images exist:** The system adds the new images to the gallery while allowing the visitor to continue browsing.
   * **If an individual image fails to load:** The system hides that image from the gallery.
   * **If no additional images exist:** The system stops loading additional batches without displaying an explicit end-of-gallery message or indicator.

---

## Flow 3: Detail View & Contextual Return

*Describes how visitors view individual images, navigate between images, and return to their previous position in the gallery.*

1. **Visitor** selects an image from the gallery.
2. **System** opens the high-resolution detail view for that image.
3. **Visitor** may navigate directly to the next or previous image in the current browsing sequence.
4. **System Evaluation:**

   * **If the visitor is viewing the first image:** The previous-image action is unavailable.
   * **If the visitor is viewing the last image:** The next-image action is unavailable.
   * **If the detail image fails to load:** The system keeps the detail view open, displays an error state, and provides a retry action.
5. **Visitor** closes the detail view.
6. **System** returns the visitor to the gallery and restores the exact grid position associated with the last viewed image.

---

## Flow 4: Image Download

*Describes downloading an image from either the gallery or the detail view.*

1. **Visitor** triggers the download action for an image.
2. **System** immediately displays an active visual indicator, such as *"Downloading..."*, to indicate that the download has started.
3. **System Evaluation:**

   * **If the download succeeds:** The system outputs the image using the exact original filename established by the Administrator and clears the active download indicator.
   * **If the download fails:** The system prevents a corrupted or incomplete file from being delivered, clears the active download indicator, and displays the exact message: *"Download failed. Please try again."*
   * **If the image was removed by the Administrator:** The download fails and the system displays the same download failure message.
4. **Visitor** may retry the download after a failure.

---

## Flow 5: Administrator Live Updates

*Describes how changes made by the Administrator affect visitors who are actively browsing.*

### Scenario A: Administrator Adds an Image

1. **Administrator** adds a new image to the live gallery.
2. **System** makes the new image available to active visitors within the defined live-update propagation target.
3. **System** places the new image at the top of the gallery's chronological sequence.
4. **System Reaction:**

   * The visitor's current viewport must not unexpectedly jump because of the new image.
   * The new image becomes available at the top of the gallery when the visitor returns to that position.
   * If the visitor is in the detail view, the new image does not alter the visitor's current detail-view navigation sequence.

### Scenario B: Administrator Removes an Image While Visitor Is Browsing the Gallery

1. **Administrator** removes an image from the live gallery.
2. **System** makes the removal visible to active visitors within the defined live-update propagation target.
3. **System Reaction:**

   * If the removed image is currently visible in the gallery, the system replaces it with a *"removed"* placeholder.
   * The removed image remains represented by the placeholder during the visitor's current browsing session.
   * After the visitor refreshes the page, the removed image and its placeholder are no longer displayed.

### Scenario C: Administrator Removes the Image Currently Open in Detail View

1. **Visitor** is viewing an image in the detail view.
2. **Administrator** removes that image from the live gallery.
3. **System** recognizes the removal within the defined live-update propagation target.
4. **System Reaction:**

   * The detail view remains open.
   * The currently displayed image remains visible until the visitor leaves the detail view.
   * The visitor's current detail-view navigation sequence is not unexpectedly interrupted.
   * If the visitor attempts to download the removed image, the download fails and the system displays: *"Download failed. Please try again."*
5. **Visitor** closes the detail view.
6. **System** returns the visitor to the preserved gallery position according to the normal detail-view return behavior.

---

## Flow 6: Empty Gallery

*Describes the visitor experience when no images have been published.*

1. **Visitor** navigates to the gallery.
2. **System** determines that no images are currently published.
3. **System** displays the exact message: *"No images available at this time."*
4. **System** does not display an image grid.

---

## Flow 7: Individual Image Load Failure

*Describes how the gallery handles an individual image that cannot be loaded.*

1. **System** attempts to display an image in the gallery.
2. **System** determines that the image cannot be loaded.
3. **System** hides the failed image from the gallery.
4. **System** continues displaying other successfully loaded images.

---

## Phase Completion Rule

This phase is complete when:

* All core public visitor journeys are represented.
* Administrator live-management interactions are represented.
* Success, failure, boundary, and empty-state behaviors are defined.
* Live additions and removals are accounted for during active browsing and detail viewing.
* The flows remain consistent with the Functional Requirements and Non-Functional Requirements.
* The flows describe user and system behavior without specifying implementation, architecture, or technology.

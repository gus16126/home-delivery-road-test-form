# Home Delivery / Rollback Road Test Assistant

A mobile-friendly, offline-capable Progressive Web Application (PWA) designed to guide examiners through daily commercial vehicle road tests for CarMax Home Delivery and Rollback drivers.

---

## 📱 Live App & Installation
The live application is hosted via GitHub Pages at:
**[https://gus16126.github.io/home-delivery-road-test-form/](https://gus16126.github.io/home-delivery-road-test-form/)**

### How to Install on your Mobile Device:
* **iOS (Safari)**: Open the link, tap the **Share** button (arrow pointing up at the bottom of the screen), and select **"Add to Home Screen"**.
* **Android (Chrome)**: Open the link, tap the three vertical dots menu, and select **"Install App"** or **"Add to Home Screen"**.

Once added, the app runs in full-screen standalone mode and functions **completely offline** (no Wi-Fi or cellular signal required).

---

## ✨ Key Features
- **63-Item Structured Checklist**: Fully formatted list covering all 12 official commercial driving test categories.
- **Yes/No Interactive Toggle Buttons**: Custom pill buttons highlight green for 'Yes' and red for 'No'. Selecting a response updates state dynamically, and only one choice can be active at a time.
- **Real-Time Progress Tracker**: Visual progress bar updates dynamically as the 63 items are answered.
- **Auto-Save Drafts**: Saves active checklist entries, driver credentials, and examiner signatures to `localStorage` in real-time. Drafts expire after 7 days.
- **Offline PWA Capabilities**: Built with a Service Worker caching assets locally to support offline execution in remote delivery areas.
- **Examiner HTML5 Canvas Signature Pad**: Responsive drawing surface for the examiner's signature directly on touchscreens (with a quick-clear option).
- **Print PDF Generation**: Custom print stylesheet formats the checklist into a clean grid, letting examiners print or save completed forms as PDFs.
- **Inspection History Log**: Saves up to 50 completed road tests with driver name, date, pass/fail status, and details for quick lookup or restoration.

---

## 🛠️ Technical Specifications & How it was Made
This project is built as a lightweight, zero-dependency single-page application (SPA).

- **HTML5 & CSS3**: Styled with CSS variable themes, responsive layout matrices, and pure CSS accordion toggles (relying on hidden checkbox selectors to expand and collapse panels).
- **Vanilla JavaScript**: Handles local storage persistence, canvas drawing math, touch/pointer cancel events, progress tracking, and Service Worker initialization.
- **PWA Configuration**: Powered by [manifest.json](manifest.json) for installation options and [sw.js](sw.js) for caching.

---

## 🔄 How to Push Updates
When you modify the checklist, follow these steps to deploy updates to users:

1. Increment the `CACHE_NAME` version string at the top of [sw.js](sw.js) (e.g., `'road-test-cache-v1'` $\rightarrow$ `'road-test-cache-v2'`). This triggers browsers to clear old caches.
2. Commit and push the changes:
   ```bash
   git add -A
   git commit -m "update: checklist changes"
   git push origin main
   ```
3. GitHub Pages will build and deploy the changes within seconds. On drivers' devices, the new version will download in the background and activate on the next launch.

---

## 📜 Changelog & Project Milestones
* **2026-06-11 (v12 - v18):**
  - **Offline PWA Core Setup:** Initialized Service Worker (`sw.js`), Manifest (`manifest.json`), and custom Steering Wheel icon (`icon.svg`) for the Home Delivery/Rollback Road Test form.
  - **Dynamic Save & Restoration:** Implemented draft saving, progress tracking, canvas signature drawing, history storage, and print styles.
  - **Side-by-Side Row 1 Layout:** Placed Driver's Name and Employee ID side-by-side with vertical labels.
  - **Inline Row 2 Date Layout:** Positioned the Date field on Row 2 with a horizontal "Date:" label to the left of the input box.
  - **Date Sizing & Safari Fix:** Configured Date field height to match Driver's Name input exactly (`40px`) and applied `-webkit-appearance: none` to prevent Safari styling defaults from breaking layouts.
  - **Employee ID Box Optimization:** Rescaled the grid flex columns from `2:1` to `3:1`. This widened the Driver/Date columns and reduced the Employee ID column by 25%, resolving right-side screen overflows.
  - **HTTP Cache Bypass:** Configured service worker fetch mechanism to request same-origin resources with `cache: 'reload'`, forcing Safari to bypass browser HTTP caches and immediately download newly deployed updates.

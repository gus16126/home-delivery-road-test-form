# Project Memory: Home Delivery / Rollback Road Test Assistant
*This document acts as a persistent memory and developer log. Any AI agent modifying this project must read, update, and respect the guidelines in this file.*

---

## 🎯 Project Context & Target User
- **User**: Gustavo Guallar (CarMax Transport Examiner / Driver)
- **Use Case**: A mobile-friendly inspection and testing guide for Home Delivery and Rollback road tests.
- **Core Requirement**: The app **must run completely offline** with zero cell signal (e.g. at highway shoulders, weigh stations, or remote dealership lots).
- **Hosting URL**: `https://gus16126.github.io/home-delivery-road-test-form/`
- **Workspace Path**: `C:\Users\trans\.gemini\antigravity\scratch\Home Delivery Road Test Form`

---

## ⚙️ AI Coding Guidelines & Architectural Constraints

### 1. Framework Constraint (Keep it lightweight)
* **Do NOT migrate this project to React, Vue, or Vite** unless explicitly requested by the user. 
* Keep it as a single-page application (SPA) centered in `index.html` with vanilla Javascript, HTML5, and CSS3. This ensures near-zero load times and perfect offline capability on older mobile devices.

### 2. Browser Compatibility (Must support Safari)
* The application must be fully tested and compatible with **iOS Safari** (the default browser used by the driver on mobile). 
* Do not use proprietary Chrome APIs. Ensure the app works seamlessly in standalone fullscreen mode when added to the iOS home screen.
* Drawing canvas must support touch event overrides (`e.preventDefault()`) with passive option set to `false` to avoid interference with Safari page bounce.

### 3. PWA & Service Worker Rules
* **Offline Caching**: Any new reference photos, scripts, or styles added to the project must be manually registered in the `ASSETS` array inside `sw.js` to ensure they are cached locally.
* **Auto-Update Cycle**: Whenever code is updated, increment the `CACHE_NAME` constant (e.g., `road-test-cache-v1` to `v2`) in `sw.js`. This notifies client browsers to download the latest files.

### 4. Styling & Accordion Logic
* **No CSS frameworks**: Maintain custom CSS variables in `:root` and styles inside `index.html`.
* **Dark Mode**: Support `@media (prefers-color-scheme: dark)` using custom colors that match Apple's dark system UI (e.g., `#121214` background, `#1c1c1e` cards, and `#0a84ff` primary tint).
* **Pure CSS Accordion**: The expand/collapse checklist steps are controlled using hidden checkbox inputs (`.step-toggle`) and sibling selectors. Avoid JavaScript logic for panel toggles.

---

## 💾 Data Schema & Local Storage Layout

### Active Progress Draft (`roadTestData`)
Saves active checkbox states dynamically to prevent data loss. Drafts expire after 7 days.
```json
{
  "driverName": "Jane Doe",
  "employeeId": "123456",
  "testDate": "2026-06-11",
  "powerUnit": "Rollback",
  "trailerType": "Cottrell",
  "passengerCarrier": "N/A",
  "examinerName": "Gustavo Guallar",
  "examinerTitle": "Examiner",
  "organizationName": "CarMax",
  "storeAddress": "123 Main St",
  "remarks": "",
  "testMiles": "15",
  "testHours": "2",
  "checks": {
    "s1-1": "yes",
    "s2-1": "no"
  },
  "signatureData": "data:image/png;base64,...",
  "timestamp": 1780860000000
}
```

### Inspection History (`roadTestHistory`)
Stores up to 50 previous completed inspections.
```json
[
  {
    "driverName": "Jane Doe",
    "employeeId": "123456",
    "testDate": "2026-06-11",
    "timestamp": 1780860000000,
    "passed": true,
    ...
  }
]
```

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

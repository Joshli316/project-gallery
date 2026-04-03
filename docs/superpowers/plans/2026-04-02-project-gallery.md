# Z's Project Gallery Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and deploy a dark-themed project gallery page for presenting 3 projects to advisors, with live demo links and Google Form feedback buttons.

**Architecture:** Single `index.html` with inline CSS, no JS framework, no build step. Screenshots captured via Playwright. Feedback collected via Google Forms. Deployed to Cloudflare Pages.

**Tech Stack:** HTML, CSS (inline), Playwright (screenshots), Google Forms, Cloudflare Pages

---

### File Structure

```
Project-Gallery/
  index.html          — the gallery page (all CSS inline)
  screenshots/
    carenavigator.png  — screenshot of CareNavigator California
    careerready.png    — screenshot of CareerReady
    uncover.png        — screenshot of Uncover
```

---

### Task 1: Capture Screenshots of Live Sites

**Files:**
- Create: `screenshots/carenavigator.png`
- Create: `screenshots/careerready.png`
- Create: `screenshots/uncover.png`

- [ ] **Step 1: Create screenshots directory**

```bash
mkdir -p /Users/zhihuang/Desktop/Projects/Project-Gallery/screenshots
```

- [ ] **Step 2: Capture CareNavigator California screenshot**

Use Playwright MCP `browser_navigate` to open `https://carenavigator-california.pages.dev`, wait for load, then `browser_take_screenshot` and save to `screenshots/carenavigator.png`.

Viewport: 1280x800. Wait for page to fully render before capturing.

- [ ] **Step 3: Capture CareerReady screenshot**

Use Playwright MCP `browser_navigate` to open `https://careerready.pages.dev`, wait for load, then `browser_take_screenshot` and save to `screenshots/careerready.png`.

Viewport: 1280x800.

- [ ] **Step 4: Capture Uncover screenshot**

Use Playwright MCP `browser_navigate` to open `https://uncover.yellow-longitudinal.workers.dev`, wait for load, then `browser_take_screenshot` and save to `screenshots/uncover.png`.

Viewport: 1280x800.

- [ ] **Step 5: Verify all three screenshots exist and look correct**

```bash
ls -la /Users/zhihuang/Desktop/Projects/Project-Gallery/screenshots/
```

Read each screenshot file to visually confirm they captured correctly.

- [ ] **Step 6: Commit screenshots**

```bash
cd /Users/zhihuang/Desktop/Projects/Project-Gallery
git add screenshots/
git commit -m "feat: capture screenshots of live project sites"
```

---

### Task 2: Build the Gallery Page

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create `index.html` with full page structure**

Write `index.html` with:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Z's Project Gallery</title>
  <style>
    /* --- Reset & Base --- */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
      font-size: 18px;
      background: #1a1a1a;
      color: #e0e0e0;
      line-height: 1.6;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }

    /* --- Header --- */
    header {
      text-align: center;
      padding: 3rem 1rem 2rem;
    }
    header h1 {
      font-size: 2.5rem;
      font-weight: 700;
      color: #ffffff;
      margin-bottom: 0.5rem;
    }
    header p {
      color: #888;
      font-size: 1.1rem;
    }

    /* --- Card Grid --- */
    .grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 2rem;
      max-width: 1200px;
      margin: 0 auto;
      padding: 0 2rem 3rem;
      flex: 1;
    }
    @media (max-width: 1024px) {
      .grid { grid-template-columns: repeat(2, 1fr); }
    }
    @media (max-width: 768px) {
      .grid { grid-template-columns: 1fr; max-width: 500px; }
    }

    /* --- Card --- */
    .card {
      background: #2a2a2a;
      border: 1px solid #333;
      border-radius: 12px;
      overflow: hidden;
      transition: transform 0.2s, border-color 0.2s;
      display: flex;
      flex-direction: column;
    }
    .card:hover {
      transform: translateY(-4px);
      border-color: #555;
    }

    /* --- Thumbnail --- */
    .card-thumb {
      position: relative;
      width: 100%;
      aspect-ratio: 16 / 10;
      overflow: hidden;
    }
    .card-thumb img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      display: block;
    }

    /* --- Status Badge --- */
    .badge {
      position: absolute;
      top: 12px;
      right: 12px;
      padding: 4px 12px;
      border-radius: 20px;
      font-size: 0.75rem;
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.05em;
    }
    .badge-live {
      background: #22c55e;
      color: #000;
    }
    .badge-beta {
      background: #eab308;
      color: #000;
    }

    /* --- Card Body --- */
    .card-body {
      padding: 1.5rem;
      display: flex;
      flex-direction: column;
      flex: 1;
    }
    .card-body h2 {
      font-size: 1.3rem;
      font-weight: 700;
      color: #fff;
      margin-bottom: 0.5rem;
    }
    .card-body .description {
      color: #ccc;
      margin-bottom: 1rem;
      font-size: 1rem;
    }

    /* --- Meta Fields --- */
    .meta {
      margin-bottom: 1.25rem;
    }
    .meta-label {
      font-size: 0.75rem;
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.05em;
      color: #888;
      margin-bottom: 0.25rem;
    }
    .meta-value {
      color: #ccc;
      font-size: 0.95rem;
    }

    /* --- Buttons --- */
    .card-actions {
      display: flex;
      gap: 0.75rem;
      margin-top: auto;
      padding-top: 0.5rem;
    }
    .btn {
      flex: 1;
      padding: 0.6rem 1rem;
      border-radius: 8px;
      font-size: 0.9rem;
      font-weight: 600;
      text-align: center;
      text-decoration: none;
      cursor: pointer;
      transition: opacity 0.2s;
    }
    .btn:hover { opacity: 0.85; }
    .btn-primary {
      background: #0ea5e9;
      color: #fff;
      border: none;
    }
    .btn-secondary {
      background: transparent;
      color: #0ea5e9;
      border: 2px solid #0ea5e9;
    }

    /* --- Footer --- */
    footer {
      text-align: center;
      padding: 2rem 1rem;
      color: #555;
      font-size: 0.85rem;
    }
  </style>
</head>
<body>
  <header>
    <h1>Z's Project Gallery</h1>
    <p>Three projects built to serve real communities</p>
  </header>

  <main class="grid">
    <!-- Card 1: CareNavigator California -->
    <div class="card">
      <div class="card-thumb">
        <img src="screenshots/carenavigator.png" alt="CareNavigator California screenshot">
        <span class="badge badge-live">Live</span>
      </div>
      <div class="card-body">
        <h2>CareNavigator California</h2>
        <p class="description">15-minute quiz that matches CA families to disability benefits and programs</p>
        <div class="meta">
          <div class="meta-label">Audience</div>
          <div class="meta-value">California families with disabled members</div>
        </div>
        <div class="meta">
          <div class="meta-label">Problem</div>
          <div class="meta-value">Navigating 100+ benefit programs is overwhelming and confusing</div>
        </div>
        <div class="card-actions">
          <a href="https://carenavigator-california.pages.dev" target="_blank" class="btn btn-primary">Try Demo</a>
          <a href="GOOGLE_FORM_URL_1" target="_blank" class="btn btn-secondary">Give Feedback</a>
        </div>
      </div>
    </div>

    <!-- Card 2: CareerReady -->
    <div class="card">
      <div class="card-thumb">
        <img src="screenshots/careerready.png" alt="CareerReady screenshot">
        <span class="badge badge-live">Live</span>
      </div>
      <div class="card-body">
        <h2>CareerReady</h2>
        <p class="description">8-tool job prep app guiding college grads from self-discovery to first job</p>
        <div class="meta">
          <div class="meta-label">Audience</div>
          <div class="meta-value">College graduates entering the job market</div>
        </div>
        <div class="meta">
          <div class="meta-label">Problem</div>
          <div class="meta-value">Job prep is scattered across dozens of tools with no clear path</div>
        </div>
        <div class="card-actions">
          <a href="https://careerready.pages.dev" target="_blank" class="btn btn-primary">Try Demo</a>
          <a href="GOOGLE_FORM_URL_2" target="_blank" class="btn btn-secondary">Give Feedback</a>
        </div>
      </div>
    </div>

    <!-- Card 3: Uncover -->
    <div class="card">
      <div class="card-thumb">
        <img src="screenshots/uncover.png" alt="Uncover screenshot">
        <span class="badge badge-live">Live</span>
      </div>
      <div class="card-body">
        <h2>Uncover</h2>
        <p class="description">Visual conversation tool for facilitators working with international students</p>
        <div class="meta">
          <div class="meta-label">Audience</div>
          <div class="meta-value">Facilitators serving international students</div>
        </div>
        <div class="meta">
          <div class="meta-label">Problem</div>
          <div class="meta-value">Facilitators need a structured way to open meaningful conversations</div>
        </div>
        <div class="card-actions">
          <a href="https://uncover.yellow-longitudinal.workers.dev" target="_blank" class="btn btn-primary">Try Demo</a>
          <a href="GOOGLE_FORM_URL_3" target="_blank" class="btn btn-secondary">Give Feedback</a>
        </div>
      </div>
    </div>
  </main>

  <footer>Z Huang &middot; 2026</footer>
</body>
</html>
```

- [ ] **Step 2: Open in browser and verify**

```bash
open /Users/zhihuang/Desktop/Projects/Project-Gallery/index.html
```

Verify:
- Page loads with dark background
- All 3 cards visible with screenshots
- Status badges show "Live" in green
- Buttons render correctly
- No console errors

- [ ] **Step 3: Test responsive layout**

Use Playwright MCP to check at 3 breakpoints:
- 1024px wide: 3-column grid
- 768px wide: 2-column grid
- 375px wide: 1-column stack

Screenshot each breakpoint for verification.

- [ ] **Step 4: Commit the gallery page**

```bash
cd /Users/zhihuang/Desktop/Projects/Project-Gallery
git add index.html
git commit -m "feat: build project gallery page with 3 project cards"
```

---

### Task 3: Create Google Forms for Feedback

**Files:**
- Modify: `index.html` (replace `GOOGLE_FORM_URL_1/2/3` placeholder hrefs with real URLs)

- [ ] **Step 1: Create CareNavigator California feedback form**

Create a Google Form titled "CareNavigator California — Feedback" with these fields:
1. "What works well?" (Long answer)
2. "What could be improved?" (Long answer)
3. "Any other comments?" (Long answer)
4. "Your name (optional)" (Short answer, not required)

Copy the shareable form URL.

- [ ] **Step 2: Create CareerReady feedback form**

Create a Google Form titled "CareerReady — Feedback" with the same 4 fields. Copy the shareable form URL.

- [ ] **Step 3: Create Uncover feedback form**

Create a Google Form titled "Uncover — Feedback" with the same 4 fields. Copy the shareable form URL.

- [ ] **Step 4: Replace placeholder URLs in `index.html`**

Replace `GOOGLE_FORM_URL_1`, `GOOGLE_FORM_URL_2`, `GOOGLE_FORM_URL_3` in `index.html` with the actual Google Form URLs from steps 1-3.

- [ ] **Step 5: Verify feedback buttons open correct forms**

Open `index.html` in browser and click each "Give Feedback" button. Confirm each opens the correct Google Form in a new tab.

- [ ] **Step 6: Commit**

```bash
cd /Users/zhihuang/Desktop/Projects/Project-Gallery
git add index.html
git commit -m "feat: add Google Form feedback links for all 3 projects"
```

---

### Task 4: Final Verification

- [ ] **Step 1: Open the complete page locally**

```bash
open /Users/zhihuang/Desktop/Projects/Project-Gallery/index.html
```

- [ ] **Step 2: Run through verification checklist**

Confirm:
- App loads correctly (dark background, 3 cards visible)
- All 3 screenshots display
- All 3 "Try Demo" buttons open correct live sites in new tabs
- All 3 "Give Feedback" buttons open correct Google Forms in new tabs
- Status badges show "Live" in green
- No console errors

- [ ] **Step 3: Test responsive at 375px, 768px, 1024px**

Use Playwright MCP to screenshot at each breakpoint. Verify:
- 375px: single column, cards stack vertically, text readable
- 768px: 2-column grid
- 1024px: 3-column grid

---

### Task 5: Deploy to Cloudflare Pages

- [ ] **Step 1: Create GitHub repo**

```bash
cd /Users/zhihuang/Desktop/Projects/Project-Gallery
gh repo create project-gallery --public --source=. --push
```

- [ ] **Step 2: Deploy to Cloudflare Pages**

```bash
cd /Users/zhihuang/Desktop/Projects/Project-Gallery
wrangler pages deploy . --project-name=project-gallery
```

- [ ] **Step 3: Verify live deployment**

Use Playwright MCP to navigate to `https://project-gallery.pages.dev` and confirm:
- Page loads correctly
- Screenshots display
- All links work
- Responsive layout correct

- [ ] **Step 4: Commit any final changes and push**

```bash
cd /Users/zhihuang/Desktop/Projects/Project-Gallery
git push
```

---

### Task 6: Loom Recording Setup

This task is done by the user (not the agent).

- [ ] **Step 1: Install Loom**

Download the Mac desktop app from https://www.loom.com/download. Sign up for a free account.

- [ ] **Step 2: Prepare your script**

For each project, plan to cover:
- What it is (one sentence)
- Who it's for
- What problem it solves
- Quick walkthrough of the live demo (click "Try Demo" from the gallery)
- What feedback you're looking for

- [ ] **Step 3: Record**

Open the deployed gallery page in your browser. In Loom:
- Select "Screen + Mic" (no camera, or camera if you prefer)
- Click Record
- Walk through each project card, click into demos, narrate

Aim for 5-10 minutes total.

- [ ] **Step 4: Share**

Send advisors:
1. The Loom video link
2. The gallery URL (`project-gallery.pages.dev`) so they can try demos and give feedback on their own

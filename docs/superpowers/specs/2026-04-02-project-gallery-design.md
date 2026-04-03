# Z's Project Gallery — Design Spec

## Overview

A single-page project gallery for presenting three projects to advisors. Advisors watch a Loom screen recording with voice narration, then can browse the gallery on their own, try live demos, and submit feedback via Google Forms.

## Projects

| Project | Status | Description | Audience | Problem | Demo URL |
|---------|--------|-------------|----------|---------|----------|
| CareNavigator California | Live | 15-minute quiz that matches CA families to disability benefits and programs | California families with disabled members | Navigating 100+ benefit programs is overwhelming and confusing | carenavigator-california.pages.dev |
| CareerReady | Live | 8-tool job prep app guiding college grads from self-discovery to first job | College graduates entering the job market | Job prep is scattered across dozens of tools with no clear path | careerready.pages.dev |
| Uncover | Live | Visual conversation tool for facilitators working with international students | Facilitators serving international students | Facilitators need a structured way to open meaningful conversations | uncover.yellow-longitudinal.workers.dev |

## Architecture

**Single `index.html`** with inline CSS and no JS framework. No build step. Deployed to Cloudflare Pages.

## Page Structure

- **Header:** "Z's Project Gallery" title
- **Body:** 3 project cards in a responsive grid
- **Footer:** Name and date

### Card Contents (top to bottom)

1. Screenshot thumbnail of the live app
2. Status badge (top-right corner, green "Live" pill)
3. Project title
4. One-line description
5. Audience
6. Problem it solves
7. Two buttons: "Try Demo" (links to live URL) and "Give Feedback" (links to Google Form)

## Visual Design

- **Theme:** Dark — background `#1a1a1a`, card background `#2a2a2a`, white text
- **Cards:** Rounded corners, subtle border, slight hover effect
- **Status badges:** Small green pill for "Live", yellow for "Beta"
- **Buttons:** "Try Demo" in teal/blue primary color, "Give Feedback" in outline/secondary style
- **Typography:** Sans-serif (Inter or system font), 18px body text
- **Responsive breakpoints:** 3 columns at 1024px+, 2 at 768px, 1 on mobile (375px)

## Screenshots

Captured from live sites using Playwright during build. Included as local image files.

## Feedback Collection

One Google Form per project with fields:
- Which project are you reviewing? (pre-filled)
- What works well?
- What could be improved?
- Any other comments?
- Your name (optional)

## Deployment

- Project folder: `~/Desktop/Projects/Project-Gallery/`
- Entry point: `index.html`
- Platform: Cloudflare Pages
- GitHub repo created during deploy

## Presentation Workflow

1. Build the gallery page (`index.html`)
2. Capture screenshots of each live app
3. Create 3 Google Forms for feedback
4. Deploy to Cloudflare Pages
5. User records with Loom (screen + voice narration over the gallery)
6. Share Loom video link + gallery URL with advisors

## Language

English only.

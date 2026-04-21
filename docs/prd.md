# Vibe Coding: Production Apps — Platform Spec

> **Purpose:** Single-file interactive learning platform for the Vibe Coding curriculum.
> No backend, no auth, fully shareable as a URL or hosted HTML file.

---

## 1. WHAT WE'RE BUILDING

A self-contained, shareable HTML learning platform that teaches developers to build
production apps using the vibe coding methodology (spec-first, Claude Code-assisted,
Next.js + Supabase + Vercel stack).

**Design principle:** One HTML file. Anyone with the link can access it. Progress
saved to localStorage — no login, no server, no friction.

---

## 2. TECH APPROACH

```
Single file:   vibe-coding-course.html
State:         localStorage (progress, tasks, notes)
Fonts:         Google Fonts CDN (Syne, DM Sans, JetBrains Mono)
Styling:       Vanilla CSS with CSS variables
Interactivity: Vanilla JS (no frameworks)
Hosting:       Any static host (GitHub Pages, Vercel, Netlify) or direct file share
```

---

## 3. DESIGN SYSTEM

```css
--bg: #0a0a0f;         /* Page background */
--surface: #111118;    /* Sidebar / card background */
--surface2: #1a1a24;   /* Elevated surface */
--border: #2a2a3a;     /* Borders */
--accent: #7c6cff;     /* Primary (purple) */
--accent2: #ff6c6c;    /* Warnings / destructive */
--accent3: #6cffb8;    /* Success / completion */
--text: #e8e8f0;       /* Primary text */
--muted: #6b6b80;      /* Secondary text */
--gold: #ffd060;       /* Callout highlights */
--code-bg: #0d0d18;    /* Code block background */

--font-display: 'Syne', sans-serif;
--font-body:    'DM Sans', sans-serif;
--font-mono:    'JetBrains Mono', monospace;
```

---

## 4. LAYOUT

```
┌─────────────────┬──────────────────────────────────┐
│   SIDEBAR       │   MAIN CONTENT                   │
│   300px fixed   │   flex-1, scrollable             │
│                 │   max-width 820px centered        │
│  [Logo]         │                                  │
│  [Progress bar] │   [Lesson Meta]                  │
│                 │   [Lesson Title]                 │
│  Phase 1        │   [Subtitle]                     │
│    01 Lesson    │   [Section Blocks...]            │
│    02 Lesson ✓  │   [Assignment Tasks]             │
│    03 Lesson    │   [Prev / Mark Complete / Next]  │
│                 │                                  │
│  Phase 2        │                          [Notes] │
│    04 Lesson    │                                  │
│    ...          │                                  │
└─────────────────┴──────────────────────────────────┘
```

---

## 5. FEATURES

### 5.1 DONE ✓

| Feature | Notes |
|---------|-------|
| Sidebar navigation | All 17 lessons, phase groups, collapse toggle |
| Progress bar | Updates as lessons are marked complete |
| Course home screen | Phase cards, stats row, CTA |
| Lesson view | Meta, title, subtitle, section blocks |
| Expandable sections | First section open by default |
| Code blocks | Syntax display, COPY button (2s reset) |
| Mark Complete | Toggle per lesson, persisted to localStorage |
| Keyboard navigation | ← → lessons, Escape → home |
| Sidebar collapse | Toggle to icon-only mode |
| All 17 lessons | Full content across 5 phases |

### 5.2 NOT YET BUILT

| Feature | Priority | Notes |
|---------|----------|-------|
| **Notes drawer** | HIGH | Floating button → slide-in panel, localStorage, per-lesson |
| **Assignment task persistence** | HIGH | Checkboxes within lessons saved to localStorage |
| **Mobile layout** | HIGH | Hamburger menu, sidebar as overlay on small screens |
| **Phase completion screen** | MED | Celebration UI when all lessons in a phase are done |
| **Lesson search / jump** | LOW | Quick-find lessons by keyword |
| **Progress export** | LOW | Copy/download a progress summary |

---

## 6. NOTES DRAWER SPEC

```
Trigger:       Floating "📝 Notes" button, bottom-right corner of main content
Behavior:      Slide in from right, overlays content (does not push layout)
Persistence:   localStorage key: notes-lesson-{id}
Keyboard:      N key toggles open/close

UI:
  - Header: "Notes — Lesson {N}" + close button (×)
  - Textarea: full height, auto-focus on open, dark styled
  - Character count (bottom right of textarea)
  - "Saved" indicator (appears 1s after last keystroke, fades)
  - No save button — autosave on input (debounced 500ms)

Width:         360px on desktop, full-width on mobile
Z-index:       100 (above content, below modal if any)
```

---

## 7. ASSIGNMENT TASKS SPEC

Each lesson section with type `assignment` renders a checklist:

```
UI:
  - Each task: checkbox square + task text
  - Checked state: green border checkbox (✓), strikethrough text
  - Persistence: localStorage key: tasks-lesson-{id} → array of checked indices
  - All checked → "Mark Lesson Complete" button pulses / glows

Behavior:
  - Click task → toggles checked
  - State survives page reload (localStorage)
  - State restored when returning to lesson
```

---

## 8. MOBILE LAYOUT SPEC

```
Breakpoint: < 768px

Sidebar:
  - Hidden by default (position: fixed, transform: translateX(-100%))
  - Hamburger button (☰) top-left of main header
  - Tap hamburger → sidebar slides in as overlay (semi-transparent backdrop)
  - Tap backdrop or lesson item → close sidebar

Main content:
  - Full width
  - Padding reduced (24px sides)
  - Code blocks: horizontal scroll maintained
```

---

## 9. CONTENT STRUCTURE (per lesson)

Each lesson contains sections in this order:

| Section | Icon | Purpose |
|---------|------|---------|
| `overview` | 🎯 | The Big Picture — why this matters |
| `mental_model` | 🧠 | The analogy that makes it click |
| `core` | 📐 | The 80/20 — 3-5 key insights |
| `example` | ⚡ | Live code / real scenario |
| `traps` | 🚫 | Mistakes that cost weeks |
| `assignment` | 🎬 | 4-6 executable tasks |

---

## 10. STATE MANAGEMENT (localStorage)

```javascript
// Keys used
'completed-lessons'   → JSON array of completed lesson IDs
'tasks-lesson-{id}'   → JSON array of checked task indices
'notes-lesson-{id}'   → string (note text)
'sidebar-open'        → boolean

// On load: restore all state
// On change: write immediately (no debounce needed for completion flags)
// Notes: debounce write 500ms after last keystroke
```

---

## 11. THE 17 LESSONS

| # | Phase | Title | Time |
|---|-------|-------|------|
| 1 | Foundation | The Vibe Coding Mental Model | 30 min |
| 2 | Foundation | Project Spec Engineering | 45 min |
| 3 | Foundation | Stack Architecture Deep Dive | 30 min |
| 4 | Scaffold | Next.js 14 App Router Mastery | 60 min |
| 5 | Scaffold | Supabase: DB + Auth + RLS + Storage | 90 min |
| 6 | Scaffold | Design System in 30 Minutes | 30 min |
| 7 | Core Build | Authentication End-to-End | 90 min |
| 8 | Core Build | Database Schema + RLS Mastery | 60 min |
| 9 | Core Build | Server vs Client Components | 45 min |
| 10 | Core Build | File Upload Pipeline | 60 min |
| 11 | Production | Admin Panel Architecture | 90 min |
| 12 | Production | Transactional Email with Resend | 45 min |
| 13 | Production | Background Jobs with Vercel Cron | 45 min |
| 14 | Production | Fintech: Payments + Compliance | 120 min |
| 15 | Ship It | Environment Management | 30 min |
| 16 | Ship It | Production Hardening + Security | 60 min |
| 17 | Ship It | Launch Checklist | 60 min |

**Total: ~20 hours of focused work**

---

## 12. BUILD ORDER (what to do next)

### Step 1 — Assignment Tasks (HIGH)
Add task checkboxes inside each lesson's assignment section.
Persist to localStorage. Restore state on lesson re-visit.

### Step 2 — Notes Drawer (HIGH)
Floating button → slide-in panel. Per-lesson, localStorage-backed.
Keyboard shortcut N to toggle.

### Step 3 — Mobile Layout (HIGH)
Hamburger + sidebar overlay. Full-width content on small screens.

### Step 4 — Phase Completion Screen (MED)
After marking the last lesson in a phase complete, show a
celebration card before advancing. Optional confetti animation.

---

*Version: 2.0 (simplified — no auth, no backend)*
*Updated: April 2026*

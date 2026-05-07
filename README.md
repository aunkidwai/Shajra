# شَجَرَة · SHAJRA

## Genealogical Family Tree Application

### Product Design & Specification Document

*Version 1.0 · May 2025*

-----

> **Document Type:** Product Specification  
> **Status:** Draft — For Review  
> **Audience:** Designers, Developers, Stakeholders

-----

## Contents

1. [Project Overview](#1-project-overview)
1. [Feature Specifications](#2-feature-specifications)
1. [Design System](#3-design-system)
1. [Interaction Design](#4-interaction-design)
1. [Responsive Design](#5-responsive-design)
1. [Data Model](#6-data-model)
1. [Technical Architecture](#7-technical-architecture)
1. [Future Roadmap](#8-future-roadmap)

-----

## 1. Project Overview

### 1.1 Purpose

Shajra (Arabic: شَجَرَة, meaning *tree*) is a web-based genealogical application designed to help families document, visualise, and explore their ancestral lineage. The application respects the cultural depth of South Asian and Middle Eastern family record-keeping traditions while delivering a modern, minimal, and accessible interface.

### 1.2 Mission Statement

> Preserve generational memory through a dignified, beautiful, and intuitive digital experience — making family heritage accessible to all ages and screen sizes.

### 1.3 Target Users

- Family historians and elders maintaining detailed lineage records
- Younger family members exploring their ancestry for the first time
- Researchers and archivists working with South Asian genealogical data
- Diaspora communities seeking to document and share their heritage

### 1.4 Core Principles

Every design and engineering decision in Shajra is guided by these four principles:

|Principle         |Definition                                                                             |
|------------------|---------------------------------------------------------------------------------------|
|**Clarity**       |Information hierarchy is always apparent. Nothing competes for attention unnecessarily.|
|**Respect**       |The interface honours the cultural weight of family memory — no gamification, no noise.|
|**Responsiveness**|Fully functional and readable on any device, from mobile phones to large monitors.     |
|**Minimalism**    |Achieve maximum meaning with minimum visual elements. Restraint is the design virtue.  |

-----

## 2. Feature Specifications

### 2.1 Tree Visualisation

The tree visualiser is the primary interface of the application. It renders an interactive SVG genealogical chart that dynamically adjusts to the structure of the family data.

|Component           |Description                                                                                                                                                                                  |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|**SVG Rendering**   |A scalable vector graph renders every family member as a named node, with edges drawn as smooth cubic Bézier curves connecting parent-to-child relationships. No external library dependency.|
|**Node Cards**      |Each node displays: full name (truncated at 18 characters with ellipsis), birth–death years or ‘living’, and a gender symbol (♂ / ♀). Root ancestor nodes have an elevated drop shadow.      |
|**Spouse Links**    |Spouse relationships are drawn as horizontal dashed gold lines at the same generational row, visually distinct from parent–child vertical edges.                                             |
|**Generation Rows** |Generational depth is automatically calculated. Each row is labelled ‘Gen 1’, ‘Gen 2’, etc. on the left margin of the canvas.                                                                |
|**Auto Layout**     |A recursive subtree-width algorithm positions nodes. Each node’s horizontal centre aligns with the midpoint of its children’s subtree, preventing overlap with no manual intervention.       |
|**Zoom Controls**   |Users can zoom in (+15% per step, max 2.5×), zoom out (min 0.3×), reset to 1×, or use ‘Expand All’ which sets zoom to 0.7× for a full family overview.                                       |
|**Selection State** |Clicking a node sets the selected state: thicker rust-coloured stroke, lighter fill. Only one node can be selected at a time. Selection triggers the detail panel.                           |
|**Deceased Styling**|Deceased members render with a muted grey fill and grey stroke, clearly distinguishing them from living members without being morbid or distracting.                                         |
|**Gender Styling**  |Male node borders use teal (`#2A6B6E`). Female node borders use rust (`#8B3A1E`). The gender icon in the top-right corner reinforces this at a glance.                                       |

### 2.2 Member Detail Panel

Selecting any node populates the right-hand detail sidebar with a structured profile of that member.

|Component           |Description                                                                                                                                                                                     |
|--------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|**Name Display**    |Full name in large serif type (26px Amiri). Arabic/Urdu name rendered below in gold, right-to-left, at 20px — respecting script direction.                                                      |
|**Metadata Rows**   |Structured rows for: Gender, Birth year, Death year (or ‘Living’), Parent, Spouse, Children count with names, Sibling count with names.                                                         |
|**Biography Block** |A styled italicised note block with a left border shows free-text biographical notes — occupation, origin, historical context, notable achievements.                                            |
|**Navigation Chips**|Clickable pill-shaped chips for all direct relations (parent, spouse, children). Clicking any chip navigates focus to that person, updating both the selection on the tree and the detail panel.|
|**Empty State**     |Before any node is selected, the panel displays a gentle prompt: *‘Select a member on the tree to view details’* — never empty or broken-looking.                                               |

### 2.3 Add Member

The Add Member workflow allows users to expand the family tree at any time without navigating away.

|Component            |Description                                                                                                                                                                                 |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|**Form Fields**      |Full Name (English), Arabic/Urdu Name, Gender (M/F select), Birth Year (number input, 1600–2030), Death Year (optional), Parent (dropdown), Spouse (dropdown), Biography / Notes (textarea).|
|**Inline Activation**|The form appears in the sidebar below the detail card. A smooth scroll-into-view animation guides the user to the form.                                                                     |
|**Dropdowns**        |Parent and Spouse dropdowns are dynamically populated from all existing family members at the time the form is opened — always up to date.                                                  |
|**Validation**       |A name is required. All other fields are optional. Submitting without a name shows a toast notification: *‘Please enter a name’*.                                                           |
|**Post-Add**         |On successful addition, the form closes, the tree re-renders with the new node, the new member is automatically selected, and a toast confirms the addition.                                |
|**Cancel**           |The Cancel button closes the form and returns the sidebar to the detail card without any data loss or side effects.                                                                         |

### 2.4 Statistics Panel

A compact statistics panel gives users an at-a-glance summary of the entire family tree data.

|Stat             |Description                                                                                                   |
|-----------------|--------------------------------------------------------------------------------------------------------------|
|**Total Members**|Count of all people currently in the dataset.                                                                 |
|**Generations**  |Count of distinct vertical depth levels in the rendered tree.                                                 |
|**Living**       |Count of members with no recorded death year — presently living.                                              |
|**Male / Female**|Two counts shown together (M / F) in a single stat box.                                                       |
|**Auto-Update**  |Statistics refresh automatically on every render cycle — whenever a member is added or the tree state changes.|

### 2.5 Legend

A compact legend at the bottom of the sidebar clarifies all visual encoding used in the tree:

- **Root Ancestor** — warm gold border (`#C49A2A`)
- **Male member** — teal border (`#2A6B6E`)
- **Female member** — rust border (`#8B3A1E`)
- **Deceased member** — grey fill and grey stroke

-----

## 3. Design System

### 3.1 Visual Philosophy

Shajra’s aesthetic is rooted in the visual language of illuminated manuscripts and historical administrative documents of the Islamic world — parchment tones, gold ink, and classical serifs — reinterpreted for a minimal digital context. The design avoids decoration for its own sake. Every visual element has a semantic purpose.

> The interface should feel like opening an old family record — familiar, warm, and trustworthy — not like opening a modern social media app.

### 3.2 Colour Palette

All colours are derived from a single warm-neutral base with two semantic accent colours. The palette is designed for extended reading comfort.

|Token             |Hex      |Usage                                              |
|------------------|---------|---------------------------------------------------|
|`--ink`           |`#111111`|Primary text, node names                           |
|`--parchment`     |`#F5EDD8`|Global background                                  |
|`--parchment-dark`|`#E8D9B8`|Card surfaces, stat boxes                          |
|`--gold`          |`#C49A2A`|Decorative accents, spouse links, generation labels|
|`--gold-light`    |`#E4C06A`|Button hover states                                |
|`--teal`          |`#2A6B6E`|Male borders, interactive accent, headings         |
|`--rust`          |`#8B3A1E`|Female borders, CTA buttons, card titles           |
|`--muted`         |`#6B5C40`|Secondary text, labels, biography text             |
|`--border`        |`#C8B07A`|All borders, dividers                              |

**Colour usage rules:**

- Parchment (`#F5EDD8`) is the global background — never white, never grey
- Gold is used exclusively for decorative accents, spouse links, and generation labels
- Teal is the primary interactive accent — links, button hovers, selected strokes, section headings
- Rust is used for female nodes, the primary CTA button, and card titles
- Pure white (`#FFFFFF`) never appears in the main interface — only in chip and button hover states

### 3.3 Typography

Two typefaces are used throughout the application, loaded via Google Fonts. No fallbacks to system fonts are acceptable — the visual tone depends on these specific choices.

|Typeface              |Role                   |Sizes Used                                                                                  |
|----------------------|-----------------------|--------------------------------------------------------------------------------------------|
|**Amiri**             |Display & Arabic script|64px (title), 26px (detail name), 22px (card titles), 20px (Arabic names), 14px (node names)|
|**Cormorant Garamond**|Body & UI              |18px (subtitle), 15px (body), 13px (chips), 12px (legend), 11px (node years)                |

**Type scale:**

|Level    |Size   |Weight |Usage                     |
|---------|-------|-------|--------------------------|
|Display  |64px   |Bold   |Page title (Amiri)        |
|H1       |40px   |Bold   |Section headings          |
|H2       |28px   |Bold   |Subsection headings       |
|Body     |15px   |Regular|All body copy             |
|Node name|14px   |Bold   |Node card names           |
|Small    |11–12px|Regular|Node years, legend, labels|

**Additional typographic rules:**

- Line height: 1.6 for body text and biography notes
- Letter spacing: `+2px` on titles, `+8px` on ornamental dividers, `+0.5px` on uppercase labels
- Node names exceeding 18 characters are truncated with an ellipsis (`…`)

### 3.4 Iconography

Shajra uses no external icon library. All icons are Unicode characters chosen for universal support and semantic clarity:

|Character|Meaning                                 |Colour|
|---------|----------------------------------------|------|
|`♂`      |Male gender indicator (node top-right)  |Teal  |
|`♀`      |Female gender indicator (node top-right)|Rust  |
|`❧`      |Floral hedera (header ornament)         |Gold  |
|`⊙`      |Reset view button                       |Muted |
|`⊞`      |Expand All button                       |Muted |

### 3.5 Surface & Texture

The background uses a repeating SVG geometric pattern — a classic Islamic four-fold grid motif — applied at 6% gold opacity. This is a purely decorative layer that adds depth and cultural context without distracting from content.

Card surfaces use `--parchment-dark` (`#E8D9B8`), slightly warmer and darker than the background, to create spatial hierarchy without harsh borders. The tree panel has a secondary inset border (1px, 40% opacity) for a refined double-border effect.

-----

## 4. Interaction Design

### 4.1 Micro-interactions

|Interaction           |Behaviour                                                                                                                                                      |
|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
|**Node Hover**        |Fill lightens to `#F0E2C0`, border transitions to gold. 200ms ease. Cursor: pointer.                                                                           |
|**Button Hover**      |200ms transition to gold background with white text. CTA darkens to `#A34524`. No bounce or scale effects — restraint is intentional.                          |
|**Chip Hover**        |Transitions to gold background with white text. Clear affordance that chips are navigable.                                                                     |
|**Toast Notification**|Slides up (`translateY: 10px → 0`) and fades in (`opacity: 0 → 1`) over 300ms. Auto-dismisses after 2800ms. Rust background, white text, bottom-right position.|
|**Form Scroll**       |On Add Member open, the page smooth-scrolls to bring the form into view — the user is never left looking for where the form appeared.                          |

### 4.2 Navigation Model

Shajra uses a single-page, no-routing architecture. All state is managed in JavaScript with no page reloads. Navigation between family members happens through:

- Direct click on any node in the SVG tree
- Clicking a relation chip in the detail panel

Both methods call the same `selectPerson()` function — consistent behaviour regardless of entry point.

> The application intentionally has no back button or navigation history. Family trees are non-linear, and users explore by curiosity rather than sequential flow.

### 4.3 State Management

The application maintains the following global state:

|Variable    |Type              |Purpose                                                                         |
|------------|------------------|--------------------------------------------------------------------------------|
|`people`    |`Array<Person>`   |All family member objects. Single source of truth for all rendering and display.|
|`selectedId`|`Integer | null`  |Currently selected person. Drives tree highlighting and detail panel.           |
|`zoom`      |`Number` (0.3–2.5)|Current zoom multiplier. Applied to SVG width and height attributes.            |
|`nextId`    |`Integer`         |Auto-incrementing unique ID for new members.                                    |


> State is not persisted between sessions in v1.0. All data exists in memory only during the page’s lifetime.

-----

## 5. Responsive Design

### 5.1 Breakpoint System

Shajra is built mobile-first with a single CSS breakpoint. The grid collapses at 900px viewport width.

|Breakpoint |Range    |Layout         |Behaviour                                                                                   |
|-----------|---------|---------------|--------------------------------------------------------------------------------------------|
|**Mobile** |`< 900px`|Single column  |Tree panel full-width, sidebar stacks below. Toolbar wraps. Font sizes scale with `clamp()`.|
|**Desktop**|`≥ 900px`|Two-column grid|Tree panel (`1fr`) + sidebar (`340px` fixed). Both panels visible simultaneously.           |

### 5.2 Mobile Adaptations

- Grid switches from `grid-template-columns: 1fr 340px` to `grid-template-columns: 1fr`
- Sidebar stacks directly below the tree panel with a 24px gap
- Toolbar buttons wrap naturally (`flex-wrap: wrap`) — no overflow or horizontal scroll
- Page title uses `clamp(36px, 6vw, 64px)` — fluid scaling from small phones to large screens
- Tree panel remains horizontally scrollable if the tree is wider than the viewport
- Card padding (20px) remains constant — adequate for touch targets
- Form inputs maintain a minimum 44px tap target height

### 5.3 Touch & Interaction

Node click events use standard `click` listeners that respond equally to mouse clicks and touch taps. There is no hover-dependent information — all hover states are purely aesthetic enhancements and their absence on touch devices does not degrade functionality.

> **Future enhancement:** Pinch-to-zoom gesture for the tree SVG on touch devices.

### 5.4 Scrollbar Styling

Custom scrollbars are defined for WebKit browsers to match the parchment aesthetic:

```css
::-webkit-scrollbar         { width: 6px; height: 6px; }
::-webkit-scrollbar-track   { background: var(--parchment-dark); }
::-webkit-scrollbar-thumb   { background: var(--border); border-radius: 3px; }
```

Non-WebKit browsers fall back to system default scrollbars gracefully.

### 5.5 Overflow Handling

The tree panel handles overflow in both axes (`overflow: auto`). This ensures that very large family trees remain navigable without breaking the page layout. The SVG dimensions are calculated dynamically and set as explicit `width` and `height` attributes on every render.

-----

## 6. Data Model

### 6.1 Person Object Schema

Each family member is represented as a plain JavaScript object:

|Field     |Type            |Required|Description                                                                            |
|----------|----------------|--------|---------------------------------------------------------------------------------------|
|`id`      |`Integer`       |Yes     |Auto-incrementing unique identifier. Used for all parent/spouse references.            |
|`name`    |`String`        |Yes     |Full name in English/Latin script. Truncated at 18 chars in node display.              |
|`arabic`  |`String`        |No      |Full name in Arabic or Urdu script. Rendered right-to-left in the detail panel.        |
|`gender`  |`'M' | 'F'`     |Yes     |Controls border colour, gender icon, and detail display text.                          |
|`birth`   |`Integer | null`|No      |Four-digit birth year.                                                                 |
|`death`   |`Integer | null`|No      |Four-digit death year. `null` indicates a living person, rendered as ‘living’.         |
|`parentId`|`Integer | null`|No      |References the `id` of this person’s parent. `null` for root ancestors.                |
|`spouseId`|`Integer | null`|No      |References the `id` of this person’s spouse. Only one side needs to define this.       |
|`bio`     |`String`        |No      |Free-text biographical notes. Rendered as an italicised note block in the detail panel.|

### 6.2 Layout Algorithm

Tree layout is computed fresh on every `render()` call using a two-pass recursive algorithm:

**Pass 1 — `getSubtreeWidth(id)`**
Recursively computes the minimum horizontal space needed by any subtree, accounting for all descendant nodes and inter-node gaps.

**Pass 2 — `place(id, x, y)`**
Assigns absolute `(x, y)` coordinates to each node. The node is horizontally centred over its children’s combined subtree. Children are placed left-to-right with `H_GAP` between them.

```
ROOT  →  placed at (0, 60)
CHILDREN  →  offset recursively
ORPHANS  →  placed to the right of the main tree
```

**Layout constants:**

|Constant|Value  |Purpose                                   |
|--------|-------|------------------------------------------|
|`NODE_W`|`150px`|Node card width                           |
|`NODE_H`|`52px` |Node card height                          |
|`H_GAP` |`30px` |Horizontal gap between sibling subtrees   |
|`V_GAP` |`80px` |Vertical gap between parent and child rows|

-----

## 7. Technical Architecture

### 7.1 Stack

Shajra v1.0 is built as a fully self-contained single HTML file with zero external runtime dependencies.

|Layer        |Technology        |Notes                                                                                |
|-------------|------------------|-------------------------------------------------------------------------------------|
|**Structure**|HTML5             |Semantic markup. SVG embedded inline.                                                |
|**Style**    |CSS3              |Custom properties, CSS Grid, Flexbox. No preprocessor.                               |
|**Logic**    |Vanilla JavaScript|No framework, no build step, no npm.                                                 |
|**Graphics** |SVG               |All tree graphics via `document.createElementNS`. Re-generated on every state change.|
|**Fonts**    |Google Fonts      |Amiri + Cormorant Garamond via single `<link>` tag.                                  |

### 7.2 Render Cycle

The application uses a simple imperative re-render pattern:

```
state mutation
    └─→  render()
            └─→  buildTree()  →  compute layout positions
            └─→  clear SVG
            └─→  draw links (parent–child, spouse)
            └─→  draw nodes
            └─→  updateStats()
```

This approach trades minor performance overhead for dramatic simplicity — no virtual DOM, no diffing, no framework overhead. For the expected data size (dozens to a few hundred family members) this is fully performant.

### 7.3 Extensibility Points

The architecture is designed to be extended in the following ways without refactoring:

|Extension           |Approach                                                                                      |
|--------------------|----------------------------------------------------------------------------------------------|
|**Data persistence**|Replace the in-memory `people` array with `localStorage`, `IndexedDB`, or a remote API call   |
|**Import / Export** |Add JSON or GEDCOM file parsing to populate the `people` array from standard genealogy formats|
|**Search**          |Add a search function that filters by name and calls `selectPerson()` on the match            |
|**Print view**      |A `@media print` CSS rule can hide the toolbar and sidebar, printing only the tree SVG        |
|**Multi-language**  |All UI strings are centralised and can be replaced with a translation object                  |

-----

## 8. Future Roadmap

### Version 1.1 — Data Persistence

- Auto-save tree data to `localStorage` on every change
- Export full tree as a downloadable JSON file
- Import tree from a JSON or GEDCOM file
- Undo/redo support for add and delete operations

### Version 1.2 — Edit & Delete

- Edit any existing member’s details inline from the detail panel
- Delete a member with cascade options (remove descendants or re-parent them)
- Drag-and-drop to re-assign a member’s parent
- Spouse unlinking

### Version 1.3 — Media & Biography

- Attach a photograph to any member — displayed in the node and detail panel
- Rich text biography with formatting support
- Attach documents (birth certificates, marriage records, wills) as file references
- Audio recordings or voice notes attached to a member’s profile

### Version 2.0 — Collaborative & Shared

- User authentication with family-scoped data access
- Shareable read-only links to a tree (public URL, no login required)
- Collaborative editing with conflict resolution
- Comment threads on individual members for family discussion
- Notification system for new additions or edits

### Version 2.1 — Advanced Visualisation

- Fan chart view (radial tree expanding outward from the root)
- Hourglass view (both ancestors and descendants from a selected person)
- Timeline view — all members laid out on a chronological axis
- Relationship path finder — *‘How is person A related to person B?’*
- Statistics dashboard with charts: life span distributions, geographic origins, generational size

-----

*❧ · End of Document · ❧*

*Shajra Product Specification · Version 1.0 · May 2025*
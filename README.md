# Smart Teams Tool

**A privacy-first, browser-native instrument for forming psychologically balanced teams using MBTI and Keirsey temperament typology.**

[![GitHub stars](https://img.shields.io/github/stars/A-Al-Tamimi/SmartTeamsTool?style=social)](https://github.com/A-Al-Tamimi/SmartTeamsTool/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/A-Al-Tamimi/SmartTeamsTool?style=social)](https://github.com/A-Al-Tamimi/SmartTeamsTool/network/members)
[![License](https://img.shields.io/badge/license-Personal%20%26%20Educational-blue.svg)](#license)
[![Live demo](https://img.shields.io/badge/demo-smart--teams--tool.com-0A0A0A.svg)](https://www.smart-teams-tool.com)

> If this work supports your research, teaching, or facilitation practice, please consider starring the repository. Visibility helps the project reach more educators and team leaders.

---

## Overview

Smart Teams Tool is a single-file, client-side web application that supports researchers, educators, and facilitators in the practical task of composing balanced groups from a roster of participants. It pairs a short, validated-style personality self-assessment with a deterministic team-balancing algorithm grounded in the Keirsey four-temperament framework (Analyst, Diplomat, Sentinel, Explorer). The tool produces interpretable, exportable artefacts — individual personality reports and full team composition documents — so that the rationale behind every team assignment is transparent and reviewable.

## Extended Summary

The motivation for this tool is methodological. Ad-hoc team formation in classrooms, workshops, and short research studies often relies on convenience or self-selection, both of which introduce well-documented biases in group dynamics, participation, and outcomes. A reproducible, type-aware allocation procedure reduces those biases by ensuring that every team contains a deliberate spread of cognitive and behavioural styles rather than a clustering of similar profiles.

Operationally, the application implements three coordinated subsystems. The first administers a 20-item forced-choice personality instrument that yields a Myers-Briggs-style four-letter type code. The second projects the 16 derived types onto the four Keirsey temperament groupings and applies an iterative swap-optimiser that maximises temperament diversity within each team while preserving optional secondary constraints such as skill-level balance and gender balance. The third renders the resulting allocation as live, interactive visualisations — radar charts of team composition, drag-and-drop participant cards, and printable PDF documents suitable for sharing with stakeholders or attaching to research records.

All computation happens entirely in the user's browser. No personally identifying information, response data, or team composition ever leaves the device, which makes the tool suitable for use in environments with strict data-handling requirements such as clinical studies, classrooms with minor participants, or organisational settings with privacy policies that prohibit third-party data processors. The application is delivered as a single static HTML file with all dependencies bundled inline, so it can be served from any static host or run directly from the local filesystem without installation, backend, or build step.

A live, hosted instance is maintained at [SmartTeamsTool.netlify.app](https://SmartTeamsTool.netlify.app) for evaluation and routine use.

---

## Features

The following features describe the current capabilities of the application. Each is implemented client-side and requires no external services.

**MBTI Personality Assessment.** The application administers a 20-question forced-choice self-assessment covering the four classical MBTI dimensions (Extraversion–Introversion, Sensing–Intuition, Thinking–Feeling, Judging–Perceiving). Items are presented one at a time with progress indication, dimension scores are accumulated transparently rather than scored through a black-box weighting, and the resulting four-letter type code is mapped to one of the sixteen MBTI personality types together with a short interpretive summary.

**Automatic Balanced Team Generation.** Given a roster of participants with assigned types, the algorithm partitions the roster into a configurable number of teams using a two-stage procedure: an initial diversity-maximising assignment based on Keirsey temperament buckets (NT, NF, SJ, SP) followed by an iterative swap optimiser that improves the global balance score while preserving per-team temperament composition. Missing temperaments are surfaced explicitly so facilitators can interpret structural gaps before committing to an allocation.

**Skill Level Tracking and Balance.** Each participant can be tagged as Beginner, Intermediate, or Advanced. When skill-balance is enabled, the optimiser treats skill distribution as a secondary objective alongside personality diversity, producing teams that are not only psychologically varied but also competence-balanced, a frequent requirement in project-based learning and mixed-ability workshop design.

**Radar Chart Visualisation.** Each team's composition is rendered as a live radar chart that plots the relative density of each temperament group and personality dimension, giving facilitators an at-a-glance read on whether a team is balanced or skewed. The radar updates in real time as members are added, removed, or swapped between teams, supporting rapid what-if exploration during a workshop briefing.

**Drag-and-Drop Reassignment.** Participants can be moved between teams by dragging cards across the team grid, with full touch-screen support for tablets used in workshop settings. Every drag updates the underlying allocation and re-renders the affected radar charts immediately, so the facilitator can iterate on the balance manually after the algorithm produces an initial split.

**PDF Report Export.** The tool produces two distinct PDF artefacts: a per-participant personality report that includes the MBTI type, dimension scores, Big Five projection, and a written interpretive summary; and a full team composition document that lists every team with its members, radar chart, and balance metrics. Both are generated entirely in the browser using html2pdf and are suitable for distribution to participants or inclusion in study documentation.

**CSV Roster Import.** Existing rosters can be imported in bulk from a CSV file, accepting columns for participant name, pre-assigned MBTI type (optional), gender, and skill level. The parser is forgiving of column order, surfaces row-level errors inline, and falls back to interactive editing for any incomplete records, eliminating the friction of retyping a known roster.

**QR Code Result Sharing.** A participant's personality result can be encoded as a QR code that, when scanned, opens a self-contained view of their report on another device. This supports lightweight handover scenarios — for example, a workshop participant scanning their own result onto their phone for later reference, without requiring an account, server, or persistent storage.

**Bilingual Interface (English and Arabic).** The full user interface, instrument, type descriptions, and exported reports are available in both English and Arabic, including right-to-left layout, mirrored controls, and appropriately localised typography. Language can be toggled at any time without losing session state, supporting bilingual classrooms and multinational research populations.

**Fully Private, In-Browser Operation.** All processing — quiz administration, scoring, team allocation, PDF generation, QR encoding — runs entirely inside the browser. No network calls are made to external services for participant data, and the application functions offline once loaded. This makes the tool appropriate for environments with strict data-residency or data-protection constraints.

---

## Live Demonstration

A hosted version is available at **[SmartTeamsTool.netlify.app](https://SmartTeamsTool.netlify.app)**. The hosted copy is functionally identical to the source in this repository.

---

## Example Roster

This repository ships with **[`example-roster.csv`](./example-roster.csv)** — a 16-participant sample covering all sixteen MBTI types with a representative mix of skill levels (beginner, intermediate, advanced) and gender entries (male, female, prefer-not-to-say). It is sized to demonstrate the application across either three or four teams and is the fastest way to see the full feature set in action.

To use it, open the application, switch to the facilitator view, choose **Import CSV**, select `example-roster.csv`, and click **Generate Teams**. With sixteen participants spread across four temperaments, the algorithm will produce balanced teams suitable for radar-chart inspection, drag-and-drop refinement, and PDF export. Try generating three teams and then four teams to compare how the optimiser distributes the same roster under different size constraints.

The CSV schema is intentionally minimal:

| Column | Required | Values |
|---|---|---|
| `name` | yes | Free-text participant name |
| `mbti` | no | One of the sixteen MBTI codes, or blank to defer the assessment |
| `gender` | no | `M`, `F`, or `NB` (also accepts `male`, `female`, and locale variants) |
| `skill` | no | `beg`, `int`, or `adv` |

You can drop in any roster that follows this header order; participants with a blank `mbti` column are flagged for in-app self-assessment before team generation.

---

## Usage

Smart Teams Tool is distributed as a single self-contained HTML file with all dependencies bundled inline. There is no installation, no backend, and no build step.

### Run from the local filesystem

```bash
# macOS
open index.html

# Linux
xdg-open index.html

# Windows
start index.html
```

### Serve locally over HTTP

Some browsers restrict certain features (clipboard, file download) when a page is opened via `file://`. Serving the file over HTTP avoids this:

```bash
# Python 3
python -m http.server 8080
# Then visit http://localhost:8080/

# Node
npx serve .
```

### Deploy to a static host

The single `index.html` file works on Netlify, Cloudflare Pages, GitHub Pages, Vercel, and any other static host. A companion `netlify/` directory in the main repository ships with the production support files needed for a polished deployment (`_headers`, `_redirects`, `robots.txt`, `sitemap.xml`, `manifest.json`, `structured-data.json`).

---

## Architecture

```
              ┌─────────────────┐
              │   Participant   │
              └────────┬────────┘
                       │   takes assessment, or is
                       │   added by a facilitator
                       ▼
              ┌─────────────────┐
              │     Roster      │
              └────────┬────────┘
                       │   Generate Teams
                       ▼
              ┌─────────────────┐    Radar chart per team
              │  Balanced Teams │◄── Drag-and-drop swaps
              └────────┬────────┘    Real-time re-balancing
                       │   Export
                       ▼
              ┌─────────────────┐
              │   PDF Reports   │
              └─────────────────┘
```

The balancing algorithm distributes participants so that each team carries the broadest possible spread of Keirsey temperament groups (NT, NF, SJ, SP). Where a complete spread is mathematically impossible (for example with fewer participants than the product of teams and temperaments), the tool surfaces the missing temperament profiles and flags the predicted team-dynamic implication so the facilitator can make an informed choice.

---

## Personality Framework

The application uses the Myers-Briggs Type Indicator four-dimension model.

| Dimension | Poles |
|---|---|
| Energy | **E**xtraversion ↔ **I**ntroversion |
| Information | **S**ensing ↔ i**N**tuition |
| Decisions | **T**hinking ↔ **F**eeling |
| Lifestyle | **J**udging ↔ **P**erceiving |

These dimensions combine into 16 types, grouped under four Keirsey temperaments: **NT** (Rationals / Analysts), **NF** (Idealists / Diplomats), **SJ** (Guardians / Sentinels), and **SP** (Artisans / Explorers).

---

## Browser Support

Modern evergreen browsers: Chrome 90+, Firefox 88+, Safari 14+, Edge 90+. No external runtime dependencies — all required libraries (PapaParse for CSV parsing, qrcodejs for QR generation, html2pdf for document export) are bundled inline.

---

## Accessibility

The interface targets WCAG 2.1 AA conformance for colour contrast across both light and dark surfaces. All interactive controls are reachable by keyboard, focus states are explicit, and ARIA labels are attached to icon-only buttons. Right-to-left layout is supported natively for Arabic, including mirrored interactive elements and locale-appropriate punctuation.

---

## License

© 2026 **Abdel-Karim Al-Tamimi**. All rights reserved.

This software is made available for personal and educational use. Commercial use, redistribution, or modification requires written permission from the author.

---

## Author

**Abdel-Karim Al-Tamimi**
LinkedIn: [linkedin.com/in/artamimi](https://www.linkedin.com/in/artamimi)
GitHub: [github.com/A-Al-Tamimi](https://github.com/A-Al-Tamimi)

---

*Built with  JavaScript, Tailwind CSS, html2pdf.js, and PapaParse. The JavaScript in distributed builds is intentionally obfuscated to protect the author's intellectual property.*

<sub><sub>© 2026 Abdel-Karim Al-Tamimi · All rights reserved.</sub></sub>

# Crash Courses

Single-file study apps for 200 level courses at the University of Lagos.
Each one is revision notes plus a practice-question bank where every answer
explains itself. No build step, no dependencies, no server-side code.

| Course | Topic | Questions | Path |
|---|---|---|---|
| **INS 202** | Human–Computer Interaction | 217 across L1–L8 | [`ins202/`](ins202/) |
| **COS 202** | Computer Programming II (Java) | 161 across 8 modules | [`cos202/`](cos202/) |
| **IFT 212** | Computer Architecture and Organisation | 279 across 11 sessions | [`ift212/`](ift212/) |
| **CSC 224** | Information Systems, Data Communication and Computer Networks | 288 across 15 modules | [`csc224/`](csc224/) |

`index.html` at the root is the hub that links to all four.

```
.
+-- index.html        # hub
+-- ins202/index.html # HCI crash course
+-- cos202/index.html # Java crash course
+-- ift212/index.html # Computer architecture crash course
+-- csc224/index.html # Information systems, data communication and networks crash course
```

## Run it locally

Serve the folder so the relative links work:

```
python -m http.server 8000
```

then open <http://localhost:8000>. (Opening `index.html` by double-click also
works — the links are relative.)

## Deploy it

Any static host. The publish directory is the repo root; the folder names
become the URL paths, so `/ins202/` and `/cos202/` resolve on their own with
no redirects or config.

**Netlify** — already connected: [![Netlify Status](https://api.netlify.com/api/v1/badges/fe9b198f-77af-4fa9-9967-c34e8b51b6ee/deploy-status)](https://app.netlify.com/projects/ins202/deploys)

**GitHub Pages** — Settings → Pages → Source: `main`, folder `/ (root)`.

## About the courses

**INS 202** follows the L1–L8 lecture slides: fundamentals, design principles
and process, conceptualizing interaction, interaction types, prototyping,
card-based prototypes, evaluation and expert reviews, usability testing.

**COS 202** follows the [Programiz](https://www.programiz.com/java-programming)
tutorial order the lecturer taught from, stopping where he stopped — at the
**Java List** section. Modules: getting started, flow control, arrays, OOP I
(classes, constructors, strings, `this`, `final`), OOP II (inheritance,
abstract classes, interfaces, polymorphism), OOP III (nested classes,
singleton, enum, reflection, packages), exception handling, and Java List
(Collections Framework, `List`, `ArrayList`, `Vector`, `Stack`).

Because it is a programming paper, a good share of the COS 202 bank is
**code tracing** — "what does this print?" — rendered in a syntax-highlighted
pane rather than as prose.

**IFT 212** follows the official work plan (Dr. C. Ojiako / Prof. F.A. Oladeji)
session by session: introduction to architecture and organisation, the von
Neumann computer model, CPU organisation, control unit design, I/O organisation
and interrupts, memory organisation and hierarchy, cache performance and
virtual memory, instruction set architecture, addressing modes and data
representation, assembly language fundamentals, and data path design and RTL.

Sessions 1–3, 5, 6, 8 and 9 are sourced from the delivered lecture series
(General Introduction, Von Neumann, Memory Org & Arch, I/O Devices, Instruction
Format & Addressing Modes) plus the compiled study notes. Sessions 4, 7, 10 and
11 are on the work plan but were not in the slide set, so they are built from
the course description, the Stallings textbook and the CSC 315 assembly
material in the course folder.

This build is **exam-first**, so it adds three things the other two do not:

- a **timed mock exam** (50 questions / 45 minutes) alongside untimed runs
- a **per-session breakdown** on the results screen after any mock, so you can
  see which session to go back to
- a **past-paper card** (`PQ`) holding the theory, register-trace and MIPS
  questions from previous papers with the skeleton of a full-mark answer for
  each — no quiz, just read it

The bank includes assembly and register-trace questions rendered in a
syntax-highlighted pane (MASM/Irvine and MIPS `lw`/`sw`).

The IFT 212 notes are written to **teach**, not just to remind — each session
opens with the problem the topic solves and an analogy, works the numbers
through by hand, and only then gives the comparison table you memorise. They
read from a standing start; the INS 202 and COS 202 notes are still in the
tighter revision-summary style.

**CSC 224** follows the 15 topics of the official course outline, in order:
systems and information systems, IS components, MIS, information processing,
input/output and storage, data security, security technologies, sorting,
searching, data communication, communication media, signals, network
classification, topologies and interconnection, and network architecture and
internet services.

Sources, all from the course folder: the outline (authoritative for module
order), the 60 KB compiled lecture notes, the Information Systems Lecture 2 and
MIS lecture notes, the Algorithms and Data Communication decks, and the 69-slide
scanned lecture deck. That last one is image-only with no text layer, so it was
read page by page; it is the reason **module 04 is the largest in the course** —
45 of its 69 slides cover processing modes (batch, real-time, OLTP with ACID,
time-sharing, distributed), which the compiled notes give only a paragraph.

Two provenance notes:

- The `CSC227 PAST_QUESTION.pdf` files in the folder are labelled **CSC 227**
  ("Introduction to Information Processing"), a different course. Checked
  against this syllabus before use: across all 154 questions there is **zero**
  coverage of OSI layers, topologies, LAN/WAN, sorting, searching, security or
  MIS/DSS, and exactly one question touching CSC 224 territory (transmission
  media). **They are not used as a source.**
- `~/Downloads/LAG-CSC224.docx` is the same document as `Notes/LAG-CSC224.docx`
  — the extracted text differs only in blank lines. It is not a separate source.

Every CSC 224 question is **original**, written against the syllabus rather than
copied from any bank, since the site is public. The notes follow the IFT 212
teaching standard.

## Notes on the build

- **Fonts.** [Quicksand](https://fonts.google.com/specimen/Quicksand) for the
  UI and [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono)
  for code, loaded from Google Fonts. If the request fails, the CSS falls back
  to the nearest geometric-rounded and monospace system faces, so the pages
  still render correctly offline.
- **Background.** The organic blobs are generated on a `<canvas>` from a
  seeded PRNG, so each screen composes its own layout and redraws identically
  on resize.
- **Progress.** Best score per module is kept in `localStorage` under
  `ins202-best` / `cos202-best` / `ift212-best` / `csc224-best` — per-browser, and it never
  leaves the device.
- **Refreshing mid-quiz.** All four courses persist the run in progress under
  `<course>-session`, so a refresh (or an accidental back-then-forward) drops
  you back on the same question rather than the home screen. What is stored is
  the question indices plus each question's shuffled option order — enough to
  rebuild the run exactly, including your position, score, missed list, and the
  answer you had already clicked. Reading notes is persisted the same way.

  The session is discarded when you finish a run, when you leave via a nav
  button, when the stored bank size no longer matches `Q.length` (i.e. the
  questions changed since), or when the JSON fails to parse — in every one of
  those cases the page simply opens on home rather than restoring something
  stale. IFT 212's timed mock stores the *absolute* deadline, so resuming shows
  the real time remaining rather than a fresh clock; a mock whose clock ran out
  while the tab was closed is dropped instead of silently resumed.
- **Narrow screens.** Wide comparison tables and code blocks scroll inside
  their own container instead of stretching the page. This depends on
  `.wrap{min-width:0}` — grid and flex items default to `min-width:auto`, which
  refuses to shrink below the content's intrinsic width, so without it a wide
  table drags the whole layout sideways and `overflow-x:auto` never engages.
  Card grids use `minmax(min(Npx,100%),1fr)` for the same reason. Verified to
  have no horizontal overflow on every module of every course from 320px up.

- **Buy me a coffee.** The hub page carries a modal (opens ~1.6s after load)
  and a matching section above the footer. Both read one constant near the
  bottom of `index.html`:

  ```js
  const COFFEE_URL = 'https://selar.com/showlove/jesutobi';
  ```

  Payments go through [Selar Show Love](https://selar.com/showlove), which
  pays out to a Nigerian bank account. Buy Me a Coffee itself is not an
  option - it routes payouts through Stripe Connect and does not list Nigeria
  as a supported payout country.

  Show Love takes a **typed amount** with a creator-set minimum, and has no
  URL parameter for presetting one. So the naira figures on the page are a
  price ladder in `.tiers`, deliberately not buttons - a clickable amount
  could neither prefill the field nor clear the minimum. The copy quotes
  1,000 as the entry point in three places: the `.tiers` list, both button
  labels, and the modal paragraph. Change the Show Love minimum and those
  need changing with it.

  Dismissing the modal writes a timestamp to `coffee-snooze` in `localStorage`
  and it stays away for 7 days (`SNOOZE_DAYS`); arriving at `/#coffee`
  suppresses it outright, since that visitor came for the ask already. The
  course pages are untouched - no popup interrupts a quiz.

## Editing the questions

The bank sits in the `<script>` block near the bottom of each course file,
one call per question:

```js
q(module, "Question text", ["A", "B", "C", "D"], correctIndex, "Why this is the answer");
```

`module` is 1–8 and `correctIndex` is 0-based. Option order is shuffled at
runtime, so answer position is never a tell. Counts in the menu are derived
automatically — add a question and the module card updates itself.

COS 202 adds a second form for questions that carry a code block:

```js
qc(module, "What does this print?", `
int i = 5;
System.out.println(i++);
`, ["5", "6", "7", "Compile error"], 0, "Post-increment returns the old value.");
```

Write the Java raw inside the backticks — `<`, `>` and `&` are escaped, then
keywords, strings, numbers and comments are highlighted at render time.

IFT 212 has the same `qc` form, highlighting assembly instead — mnemonics,
registers (including `$t0`-style MIPS names), numeric literals with MASM radix
suffixes, `.DIRECTIVES` and `;` comments.

Notes for each module live in the `NOTES` object just below the bank, keyed
1–8 (1–11 plus `pq` for IFT 212). The module cards themselves come from the
`MODULES` array above it; an entry with `paper: true` renders a read-only card
with no quiz button, which is how the IFT 212 past-paper section works.

Timed runs are opt-in per button: `data-mock="50" data-mins="45"` on the mock
card starts a 50-question run with a 45-minute countdown. Omit `data-mins` and
the run is untimed. The countdown only ever calls `finish()`, so a timed run
ends through the same path as one you complete by hand.

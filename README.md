# Crash Courses

Single-file study apps for 200 level courses at the University of Lagos.
Each one is revision notes plus a practice-question bank where every answer
explains itself. No build step, no dependencies, no server-side code.

| Course | Topic | Questions | Path |
|---|---|---|---|
| **INS 202** | Human–Computer Interaction | 217 across L1–L8 | [`ins202/`](ins202/) |
| **COS 202** | Computer Programming II (Java) | 161 across 8 modules | [`cos202/`](cos202/) |
| **IFT 212** | Computer Architecture and Organisation | 279 across 11 sessions | [`ift212/`](ift212/) |

`index.html` at the root is the hub that links to all three.

```
.
├── index.html        ← hub
├── ins202/index.html ← HCI crash course
├── cos202/index.html ← Java crash course
└── ift212/index.html ← Computer architecture crash course
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
  `ins202-best` / `cos202-best` / `ift212-best` — per-browser, and it never
  leaves the device.

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

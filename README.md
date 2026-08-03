# Crash Courses

Single-file study apps for 200 level courses at the University of Lagos.
Each one is revision notes plus a practice-question bank where every answer
explains itself. No build step, no dependencies, no server-side code.

| Course | Topic | Questions | Path |
|---|---|---|---|
| **INS 202** | Human–Computer Interaction | 217 across L1–L8 | [`ins202/`](ins202/) |
| **COS 202** | Computer Programming II (Java) | 161 across 8 modules | [`cos202/`](cos202/) |

`index.html` at the root is the hub that links to both.

```
.
├── index.html        ← hub
├── ins202/index.html ← HCI crash course
└── cos202/index.html ← Java crash course
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
  `ins202-best` / `cos202-best` — per-browser, and it never leaves the device.

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

Notes for each module live in the `NOTES` object just below the bank, keyed 1–8.

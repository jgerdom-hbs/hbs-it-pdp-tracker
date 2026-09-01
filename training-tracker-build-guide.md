# Building a training tracker page from a CSV

This is the spec behind the FY27 training page, written so it can be rebuilt
from a CSV in any future year, by anyone, with any AI assistant.

## 1. The CSV

Six columns, one row per training item:

| Column | Meaning |
|---|---|
| `Quarter` | Short label for the circle marker — Q1, Q2, Q3, Q4 |
| `DateRange` | The subtitle under the quarter label, e.g. `Jul - Sep 2026` |
| `Sequence` | Order within the quarter — 1, 2, 3... |
| `Title` | The item name, as it should display |
| `Category` | The pill text — LinkedIn Learning, Anthropic Academy, OpenAI Academy, Practice, or any new category |
| `URL` | Link target. Leave blank for items with no link (e.g. practice/behavioral goals) |

Rows with the same `Quarter` group together; `Sequence` controls the order
inside that group. See `training-plan-template.csv` for a filled-in example.

## 2. Category colors

Each distinct `Category` gets its own pill: a light tint background paired
with a saturated version of the same color as the text — never two different
hues in one pill. The palette below is one option (it happens to be
Anthropic's own colors); swap in different hues freely, the pairing pattern
is what matters:

| Category | Background | Text |
|---|---|---|
| LinkedIn Learning | `#e8f0f8` | `#6a9bcc` |
| Anthropic Academy | `#f5ddd0` | `#d97757` |
| OpenAI Academy | `#eceae4` | `#6b6a63` |
| Practice | `#eef1e9` | `#788c5d` |

New category shows up in the CSV that isn't listed here → pick any unused
hue and apply the same light-tint-background-plus-matching-text rule.

## 3. Page structure

- **Header**: title, one-line subtitle, a circular progress ring showing
  percent complete, and a count ("X / Y items complete") computed from the
  total row count and how many are checked.
- **Filter dropdown**: "All" plus one option per distinct `Category` in the
  CSV. Selecting one hides every item whose category doesn't match, and
  collapses any quarter left with nothing visible.
- **Trail**: one row per distinct `Quarter`, in order. Each row has a
  circular node (labeled with the `Quarter` value), a connecting line to the
  next quarter, and a heading showing that quarter's `DateRange`. Under the
  heading, a checklist of that quarter's items in `Sequence` order.
- **Item**: a checkbox, the `Title` (as a link if `URL` is present), and the
  `Category` pill directly below the title, left-aligned with it.
- **Footer**: a reset button that clears all checked state.

Checking a box updates: that item's checked style, the quarter node's fill
percentage, the connecting line (solid once the quarter is 100% done), and
the overall progress ring — plus saves the new state.

## 4. Building it — Claude

Claude Artifacts can persist data between visits with a built-in
`window.storage` API — no setup needed, and nothing else can read it.

**Prompt to paste into Claude**, along with the CSV attached:

> Build an HTML artifact from the attached CSV using the structure and
> category colors in this guide. Use `window.storage.get`/`.set` (personal,
> not shared) to save checkbox state between visits, keyed under a single
> JSON blob. Wrap storage calls in try/catch so it still works if storage
> is unavailable.

## 5. Building it — ChatGPT or any other assistant

The only Claude-specific piece is `window.storage`, since it doesn't exist
outside Claude's artifact environment. Everywhere else, swap in the
browser's standard `localStorage`, which works in any HTML file opened
directly or hosted anywhere:

```js
// Claude version
await window.storage.set('progress', JSON.stringify(state), false);
const result = await window.storage.get('progress', false);

// Platform-agnostic version
localStorage.setItem('progress', JSON.stringify(state));
const raw = localStorage.getItem('progress');
```

**Prompt to paste into ChatGPT (or similar), along with the CSV attached:**

> Build a single self-contained HTML file from the attached CSV using the
> structure and category colors in this guide. Use `localStorage` to save
> checkbox state between visits, keyed under a single JSON blob, wrapped in
> try/catch. No external dependencies other than Google Fonts.

One caveat worth passing on: `localStorage` is tied to the exact file
location it's opened from. If someone re-downloads or moves the file, their
saved progress won't follow it — that's the trade-off for not needing
Claude's artifact environment.

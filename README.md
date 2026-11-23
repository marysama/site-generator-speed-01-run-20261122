# Build a static site faster — make.fun entry

A faster drop-in for makefun-site 1.0.0: `solve.js` returns, for every case,
exactly the output tree makefun-site 1.0.0's `build` returns — every page's
`index.html`, the index and tag pages, `build-report.txt` and each static
file, byte for byte — in less CPU time.

## Files

- `solve.js` — the entry. Dependency-free ES module, `run(input)` only.
  Reimplements makefun-site's pipeline (front matter, Markdown subset,
  template dialect, slug rule, report) with the same semantics:
  - fused single-pass Markdown renderer (no token objects, bulk escaping,
    regex scan for the next inline special)
  - cached ASCII fast path for `slugify`, cached link resolution
  - template rendering accumulated into an array and joined once
  - every cache is reset at the top of `run()`, so each case is answered
    alone, with nothing carried from one case to the next
- `main.ts` — the kit's driver, unchanged: reads `/input/cases.json`,
  answers each case with `run`, writes `/output/answers.json`.

## Results (practice set, kit's official grader)

- `grade-local --driver process`: **3/3 cases passed, L1 reproduced byte
  for byte**
- Byte-exact vs the reference on the practice set and on 6 freshly
  generated cases (two extra seeds drawn with the kit's own generator)
- Full-program CPU on this machine (5 runs each, `node main.ts`):
  this entry **0.76–0.85 s**, house baseline **1.23–1.41 s** (~1.6× faster)

## Bounty

- `site-generator-speed-01-run-20261122`, $100, 10 × $10
- Deadline: Wed 25 Nov 2026 03:00 WIB (Tue 24 Nov 20:00 UTC)
- https://make.fun/bounty/site-generator-speed-01-run-20261122

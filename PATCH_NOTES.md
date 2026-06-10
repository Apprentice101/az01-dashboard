# Updates Panel Enhancement — Patch Notes

## What changed
The right-rail **Updates** panel on the AZ-01 dashboard now merges three
review streams into one prioritized queue:

1. **Conflicts** (red pill) — from `data.CONFLICTS`, unresolved first
2. **Updates** (amber pill) — from `data.UPDATES`, the original cards
3. **News** (blue pill) — from `data.NEWS`, newest first

Each card shows a color-coded type pill, a one-line reason summary, source
attribution, and a dismiss × button.

## Header count
The panel header now reads:

    Updates   N updates · N conflicts · N news

Counts decrement live when a card is dismissed.

## Files added
- `patch-updates-panel.css` — styles for type pills, accent stripes, reason
  lines, injected card shell, and dismiss button.
- `patch-updates-panel.js` — runs after React mounts; decorates existing
  Update cards in place and injects Conflict/News cards from `snapshot.json`.
  Idempotent; re-applies on React re-renders via MutationObserver.
- `index.html` — two added lines just before `</head>`:

  ```html
  <link rel="stylesheet" href="./patch-updates-panel.css">
  <script defer src="./patch-updates-panel.js"></script>
  ```

## Files NOT changed
- `assets/*` (built React bundles)
- `snapshot.json`
- `README.md`

## Behavior notes
- Real `<Update>` cards keep their existing React-driven dismiss × button
  (unchanged). Injected Conflict/News × buttons hide the card locally and
  persist the dismissal in `sessionStorage` (cleared on tab close).
- The patch is pure DOM and does not touch React state.
- If the React app re-renders the Updates panel, the patch re-applies
  automatically via a MutationObserver.

## Removing the patch
Delete the two new files and the two lines added to `index.html`. The
dashboard reverts to its prior behavior with zero residual effects.

---

## 2026-06-10 data refresh

In addition to the UI patch, `snapshot.json` was refreshed to **data as of
2026-06-10**:

- `dataAsOf`, `updatedAt`, `exportedAt` bumped to 2026-06-10
- `LAST_REFRESH` recorded as a manual run on 2026-06-10
- `NEXT_REFRESH_AT` set to 2026-06-12
- `WEEKLY_REPORT` rolled to week 24 (Jun 8–14)
- 6 new NEWS items added (Jun 5–9), older items demoted to `reviewState=reviewed`
  to keep the unreviewed queue manageable:
  - Axios — DCCC/NRCC pick candidates in competitive CD1 race
  - KJZZ — Outside counsel to investigate "theft" claims against Heap's office
  - Democracy Docket — Maricopa BOS vs. Heap election-equipment dispute
  - KJZZ — Former Recorder Helen Purcell backs Board in election fight
  - Votebeat — AZ Supreme Court declines fake electors review
  - The Voting News — Jun 17 special session on redistricting & QR vote counting
- No ELECTION_EVENTS, RULES, RESOURCES, CONFLICTS, or SOURCES changed
- Primary remains **July 21, 2026** (41 days out as of 2026-06-10)

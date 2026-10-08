# FY27 PDP Tracker Handoff

Last updated: Oct. 7, 2026

## Summary

The FY27 training plan is now a Claude artifact. It saves your items and checkmarks to your Claude account, so the same list shows on every computer where you sign in to Claude.

**Live tracker:** https://claude.ai/artifact/NEqAj9i2YMUMwwu5zU5sAP

## What Was Done

| Item | Detail |
|---|---|
| Built the artifact | Same look as the old page. Uses the same 11 items, four quarters, and four categories. |
| Added list editing | **Edit list** mode adds, edits, reorders, and deletes items. You can add new categories. |
| Set up saved data | Items and checkmarks save to the artifact's database. Only you can change them. |
| Loaded the items | All 11 items were loaded and read back to confirm. All start unticked. |
| Saved the source | `pdp-tracker.html` was committed (`ccc19a2`) and pushed to GitHub. |

## How It Is Built

| Part | Where it lives | What it holds |
|---|---|---|
| The page | `pdp-tracker.html` in this repo, plus the published copy on claude.ai | Layout, colors, and button behavior |
| Your data | The artifact's database, document `plans/fy27` | Quarters, categories, items, and checkmarks |

The HTML file holds no items or checkmarks. If you delete the artifact, you lose the data. The file can rebuild the page only.

## How To Update It

### Tick an Item or Change the List

1. Open the tracker link.
2. To mark an item done, select its checkbox.
3. To change the list, select **Edit list**.
4. Use **+ Add item**, **Edit**, **Delete**, or the **↑ ↓** buttons.
5. Select **Done editing**.
6. Make sure the status line says "Saved to your account."

### Change the Page Design

1. Start a Claude Code session in this folder.
2. Tell Claude what to change on the page.
3. Claude edits `pdp-tracker.html` and publishes it to the same link.
4. Your items and checkmarks stay the same.

### Change Items Through Claude

1. Give Claude the tracker link.
2. Tell Claude what to add, change, or remove.
3. Claude updates the `plans/fy27` document directly.

## What Is Still Open

| Item | Status |
|---|---|
| Tick finished items | Your old checkmarks did not carry over. Tick the items you already finished. |
| Test edit buttons on the live page | Not tested yet. Try one add, one edit, and one delete. |
| Test on a second computer | Not tested yet. Open the link elsewhere and confirm your ticks show. |
| Pin to sidebar | Optional. Ask Claude to pin it. |
| Old files | `fy27-training-development-plan.html` and `training-tracker-build-guide.md` still describe the old saving method. You chose to leave them as they are. |
| Same-time edits | If you edit on two computers at the same moment, the last save wins. |
| Quarter labels | The page can't edit quarter names or dates. Ask Claude to change them. |

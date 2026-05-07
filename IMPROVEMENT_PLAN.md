# UI Improvement Plan

Design review of `index.html`, grouped by priority and area.

## High-impact

1. **DONE.** **Per-item play button.** Currently the only way to start playback is "Let's Jam" (whole list). Add a primary "Play" action on each item so users can jump straight to one video — keeps Edit/Remove as secondary.
2. **DONE.** **Drag handle is invisible.** `cursor: grab` is the only affordance. Add a small ⋮⋮ handle on the left of each row so reorder/move-to-other-list is discoverable. Right now there's no visual cue that rows are draggable, especially for moving onto the sidebar.
3. **Visual hierarchy is flat.** The gradient "Let's Jam" button is the only saturated element on the page, so it dominates while everything else (titles, badges, form) blends together. Either pull the gradient back to a solid accent, or push more accent color into the active playlist, primary buttons, and the H1, so attention isn't pinned to one button.
4. **Sticky player + page title fight for the top.** When the player opens, `<h1>Youtube Jam Buddy</h1>` and the add form are pushed below the video. Consider moving the H1 + add form into a slim top bar (or into the sidebar header) so the playlist stays in view while the sticky player is active.

## Sidebar

5. **DONE.** **Active vs hover states are nearly identical** — both use `--panel` bg; only the 1px accent border distinguishes active. Make active more obvious (left accent stripe, bolder text, or filled accent bg with white text).
6. **DONE.** **Count `(3)` in muted gray** reads like a typo. A small pill (`background: var(--border); border-radius: 999px`) right-aligned in the row would feel more like a list count.
7. **"Tools" row** with dashed Export/Import buttons feels secondary-but-noisy. Try icon buttons (↑ Export / ↓ Import) in a single tighter row, or tuck them behind a "···" overflow on the playlist title.
8. **No "delete playlist" affordance in the sidebar.** Today it's only on the active list header. A right-click / hover-revealed × on each sidebar row would be more conventional.

## Playlist items

9. **Speed/volume badges** look identical (same pill, same color). Color-code them — e.g. speed in accent, volume in muted — or prefix with an icon (♪ / 🔊) so they scan at a glance.
10. **DONE.** **Time-range row** (`00:01:30 → end`) is buried below the URL. Promote it next to the title; the URL is the least useful field and could be hidden behind a hover/expand.
11. **DONE.** **Card padding feels tight.** With a 240px thumbnail and 12px padding, titles wrap into a cramped column. Bump padding to 16px and increase row gap.
12. **DONE.** **Edit form** stacks Title → Speed → Time row → Volume → actions. Volume between time and actions feels arbitrary. Group as: Title, then a row of (Speed + Volume), then Start/End, then actions.

## Player bar

13. **DONE.** **Controls will wrap awkwardly** on narrower main columns: Speed select + volume slider + Prev + Next + Stop. Either drop labels to icons on small widths, or move secondary controls (speed, volume) into an overflow.
14. **DONE.** **"Now jamming"** label never updates with the actual title — `playerNowPlaying` is set once. Show the current track name there; that's prime real estate.
15. **No progress indicator.** Even a thin time bar under the iframe header (current / start→end) would help loop-practice users see where they are within the clipped range.

## Polish

16. **No brand mark.** A small SVG (guitar pick, waveform, play triangle) next to the H1 would give the app some personality — right now it reads like a generic admin tool.
17. **Subtitle "Saved locally in your browser"** is fine info but eats prime space below the H1. Move to a small footer or a tooltip on a "Local" badge.
18. **Status row** reserves 20px even when empty. Use a toast/inline-on-form-row instead so the layout doesn't have a permanent gap.
19. **Mobile (≤800px)** dumps the whole sidebar above the playlist — that's a lot of scrolling before users see videos. Consider a collapsed "Playlists ▾" disclosure or a slide-in drawer.
20. **Keyboard shortcuts** aren't surfaced. Space = play/pause, ←/→ = prev/next, with a small "?" key hint in the player header would feel pro.

## Code quality, performance, accessibility & maintenance

### Code quality

21. **DONE.** **Stale items in this doc.** #1 (per-item play), #2 (drag handle), #5 (active-state stripe), and #14 (now-playing label) are already implemented — `playerNowPlaying.textContent = v.title` runs in `loadJamItem` at index.html:1458. Mark done or remove.
22. **DONE.** **Duplicated "sync working copy into active list" block** appears 5×: index.html:717-721, 914-918, 933-937, 1576-1580, 1599-1603. One `syncActive()` helper would remove a class of bugs where someone mutates `lists` without flushing `videos`/`listTitle` first.
23. **Implicit aliasing between `videos` and `lists[i].videos`** (same array reference). It works, but switching/creating/importing has to remember to reseat the reference. A pure "active list selector" would be safer.
24. **Inconsistent error handling.** `verifyVideo` throws (index.html:807), `saveList` returns `false` (index.html:716), drag/player handlers swallow errors with `catch (_) {}` in 6 places (e.g. 1368, 1457, 1492, 1506, 1680). Real player failures are invisible.
25. **`extractVideoId` doesn't validate the 11-char ID shape** (index.html:785). `?v=foo` is happily passed to oEmbed, which fails with a generic "Video not found".
26. **`render()` re-runs on every speed/volume change** (index.html:1668, 1685) even when only a single badge changed — full list teardown for a one-character update.
27. **DONE.** **Add-flow has no fetch timeout/abort** (index.html:810). If oEmbed hangs, the Add button stays disabled until reload.

### Performance

28. **Whole-list DOM rebuild on every state change** via `listEl.replaceChildren()` + loop (index.html:875-884). Fine at 10 items, noticeable at 200+.
29. **DONE.** **Inline styles set imperatively in the edit form** (index.html:1236-1238, 1252-1254, 1268-1271) — same 3-line flex block 3×; belongs in CSS. *Added `.edit-form label.edit-field` rule and replaced the 9 inline style assignments with `className = "edit-field"`.*
30. **`document.querySelectorAll(".drop-above, .drop-below, ...")` on every dragover** (index.html:1106). `dropTargetId` already tells you which row to clear.
31. **`localStorage.setItem` on every micro-change** — volume slider `change`, speed select, drag drop, add, edit each writes the whole `lists` blob. With many/large playlists, a debounced or batched save would help.

### Accessibility

32. **DONE (intra-list only).** **Drag-and-drop is mouse-only.** No keyboard alternative for reorder or move-to-other-playlist. Biggest a11y gap. *Keyboard reorder within the active list is now supported (Space / ↑↓ / Esc on the drag handle); cross-playlist keyboard move still TODO.*
33. **DONE.** **`#status` has no `aria-live`** (index.html:639). Screen readers never hear "Verifying video…", error toasts, or import success.
34. **Sidebar `<aside>` and playlist `<ul>` have no `aria-label`** — landmarks are unnamed.
35. **Volume slider's accessible name is just "Vol"** (index.html:612-616). The `<span id="jamVolumeValue">` showing the % is outside the label, so SRs don't announce the current value. Use `<output for>` or fold the value into `aria-valuetext`.
36. **No focus management after `deleteList` / `switchActiveList`** — focus falls back to `<body>` because the focused button just got removed.
37. **Drop indicators are color-only** (index.html:313-314). Not catastrophic since drag is mouse-only, but users with low contrast vision may miss them.
38. **`confirm()` for destructive delete** (index.html:957) — functional but unstyled and not announced as a dialog.

### Maintainability

39. **DONE.** **Spec drift (also called out in CLAUDE.md).** The spec describes Playback / Edit / Edit-Video as discrete modes; the app collapses them into one combined view. Either update the spec or implement the modes — right now the source of truth is ambiguous. *Resolved by rewriting the "App Modes" section of REQUIREMENTS.md to describe the actual combined view + inline-edit UX.*
40. **DONE.** **One 660-line `<script>` block with 13 module-level mutable globals** (`videos`, `listTitle`, `lists`, `activeId`, `loopPlaylist`, `editingId`, `editingTitle`, `draggedId`, `dropTargetId`, `dropPosition`, `jamPlayer`, `jamQueue`, `jamIndex`). Single-file is a hard constraint, but the code can still be grouped into clearly-marked sections (state / storage / render / drag / player / io) with section banners. *Banners added: State, Storage helpers, IFrame API loader, Time helpers, URL parsing, Existence check, DOM refs, Overflow menu, Render, Keyboard reorder, Drag and drop, View / edit forms, Playback, Top-level button bindings, Import / export, Player control bindings, Add flow.*
41. **DONE.** **No JSDoc on the storage shape.** The on-disk schema must be reconstructed by reading `loadState`. A single typedef at the top would document it in one place. *Added `JamVideo` / `JamList` / `StorageShape` typedefs at the top of the script.*
42. **DONE.** **Drag state isn't cleared on Escape.** If the user starts a drag and presses Esc, `draggedId` stays set until the next dragend. *Extended the existing global Escape handler to clear `draggedId`, drop indicators, and the `.dragging` class.*
43. **Same-video-across-playlists isn't deduped.** Add-time uniqueness check (index.html:1712) only looks at the active list; "move to other list" can produce duplicates if the user re-adds elsewhere. Probably intentional, but undocumented.
44. **This doc itself is untracked and partly outdated.** Either commit it, mark items done, or migrate live items to issues.

## Top picks

Biggest bang-for-buck: **#1 (per-item play)** ✅, **#2 (drag handle)** ✅, **#5 (active sidebar state)** ✅, **#14 (real now-playing title)** ✅.

From the new section: **#33 (`aria-live` on status)** ✅, **#32 (keyboard reorder)** ✅ (intra-list), **#22 (extract `syncActive()`)** ✅, **#27 (fetch timeout)** ✅.

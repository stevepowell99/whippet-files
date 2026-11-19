Whippet thumbnails keep stopping after a while and come back on restart. Cause not yet known; logging now in place to find it.

## Ruled out (19 November 2026)

- A hung shell call (the 8 s timeout, 47c7f37): no `thumbnail timeout` line in the perf log after thumbnails stopped again.

## Measured (19 November 2026, ~12:20)

| File type | Fresh Electron process, same moment |
|---|---|
| .png, .jpg, .mp4 | thumbnail OK |
| .pdf, .docx, .xlsx, .pptx | fail at once, "local thumbnail cache reference" |

- PDFs: no thumbnail handler registered on this machine, so they only ever show one Windows had cached. Not a Whippet fault.
- Office files: thumbnail only if saved with a preview picture. Not a Whippet fault.
- **.png and .bmp go through Google Drive's provider** (`drivefsext.dll`, `DisableProcessIsolation=1`), loaded inside Whippet's main process; .jpg uses Windows' own Photo Thumbnail Provider.
- Drive for Desktop restarted at 07:32 on 19 November; Whippet's main had been running since 07:20 on 18 November.

## Lead

- Drive's provider, loaded in Whippet before a Drive restart or standby, loses its link to Drive; a fresh process works.
- Predicts: .png and screenshots lose thumbnails, .jpg photos keep them.

## Next

- Steve: press Restart in the search box (main.js now logs image-thumbnail failures).
- Next time thumbnails go: say which kinds went (screenshots/.png, photos/.jpg, PDFs, Office).
- Claude: read `thumbnail failing <ext>` / `thumbnail ok again` lines in `%TEMP%\tagfox-mainperf.log`.
  - Only .png/.bmp failing: lead confirmed. Fix: make image thumbnails without the shell (`nativeImage.createFromPath` + resize), so Drive's provider is never loaded.
  - .jpg failing too: the shell itself in this process; consider making thumbnails in a separate process that is replaced when it fails.
  - No lines at all while thumbnails are missing: main answered fine, so the fault is in the renderer (the lazy-load observer, `thumbCacheGet`).
- Delete this file once thumbnails have survived a standby and a Drive restart.

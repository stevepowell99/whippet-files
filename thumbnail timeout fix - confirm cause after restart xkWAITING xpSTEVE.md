Whippet thumbnails stopped showing on 18 November 2026; fix 47c7f37 (8 s timeout per shell thumbnail call) is unproven.

- Cause is a hypothesis: the serialised thumbnail queue in `main.js` wedged behind one hung `createThumbnailFromPath` after a standby (perf log showed 49 s and 88 s lags at 21:52 on 17 November).
- Steve: restart Whippet (Restart in the search box); say whether thumbnails return.
- If they stop again: look for `thumbnail timeout` lines in `%TEMP%\tagfox-mainperf.log`.
  - Present: hypothesis confirmed, consider why the shell call hangs after standby.
  - Absent: the cause is elsewhere (renderer side, `thumbCacheSet`/cache of failures).
- Delete this file once thumbnails have survived a standby.

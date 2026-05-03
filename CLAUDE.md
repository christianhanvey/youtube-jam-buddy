# Youtube Jam Buddy

Single-file SPA for building YouTube playlists tuned for jamming/practice (custom start/end times, per-video playback speed and volume).

## Shape of the project

- Everything lives in `index.html` — HTML, CSS, and JS in one file. No build step, no bundler, no dependencies.
- All state is stored in `localStorage` under the key `yjb.lists.v1` as `{ activeId: string, lists: [{ id, title, videos }] }`. The `videos` and `listTitle` module-level vars are working copies of the active list — `saveList()` syncs them back into the active list entry before writing. No backend, ever — that's a hard requirement from the spec.
- The only external runtime dependency is the YouTube IFrame API, loaded on demand when the user starts the jam player.
- Video metadata is fetched via the public YouTube oEmbed endpoint (no API key needed).

## Running locally

```
python3 -m http.server 8080
```

Then open `http://localhost:8080`. There is no test suite, no linter, no CI.

## Files

- `index.html` — the entire app.
- `REQUIREMENTS.md` — the product spec. The bottom of the file contains sample YouTube URLs and sample playlist JSON used as test fixtures; ignore those sections when reasoning about app behavior.

## Conventions

- Keep it a single file. Don't introduce a build step, npm, or a framework without explicit ask.
- Don't add a backend, server-side storage, or anything that requires a YouTube API key.
- Persist state through `saveList()` so quota-exceeded errors are handled and the in-memory state stays consistent with `localStorage`.

## Known gaps vs. the spec

The spec describes explicit Playback / Edit / Edit Video modes. The current code shows the playlist and its editing controls as one combined view — there's no mode toggle. Multi-playlist support (sidebar with create/switch/delete) is implemented.

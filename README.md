# Youtube Jam Buddy

A single-page web app for building YouTube playlists tuned for jamming and practice.

Paste any YouTube URL to add it to a playlist, then hit **Let's Jam** to play through the videos in sequence. Each video can be customised with a playback start time, end time, speed, and volume — useful for skipping intros, looping a tricky bridge, or slowing down a fast passage to learn it.

## Features

- Multiple playlists, each with its own title — switch between them from the sidebar, which also shows each playlist's video count.
- Per-video custom start/end times (`HH:MM:SS`), playback speed (0.25× – 2×), and volume.
- Drag-and-drop to reorder videos within a playlist, or onto another playlist in the sidebar to move a video between lists.
- Sequential playback with prev/next controls and a stop button.
- Everything is saved in your browser — no account, no backend, no API key.

## Running

It's a single static HTML file. Serve it with anything; the simplest:

```
python3 -m http.server 8080
```

Then open <http://localhost:8080>.

## Project layout

- `index.html` — the entire app (HTML, CSS, JS).
- `REQUIREMENTS.md` — the product spec.
- `CLAUDE.md` — notes for working on the project with Claude.

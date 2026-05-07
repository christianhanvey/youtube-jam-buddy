# Project Name: Youtube Jam Buddy
# Project Type: Web

# Project Description:
A Single Page Application style website that allows you to create a playlist from youtube videos and play the list back, using the Youtube API.
The goal is to facilitate learning, practice of a musical instrument, or simply enjoyment of playing along with music, by playing a list of videos that can have playback speeds set to comfortable levels.
If the musical section required in a video is in the middle, the user can adjust the start and end of playback points to skip unnecessary parts of the video
When the list is built, a single 'play all' option should allow playback of all videos in sequence, appying any custom attributes that were previously set.
All settings and modifications to the list should be stored in the browser so if the page is reloaded, or revisited, the page is automatically repopulated


# Concepts

## Jam Playlist
- A Jam Playlist is a simple list of jam videos
- A Jam Playlist can be given a title
- A user can have multiple jam playlists

## Jam Video
- Each jam video has the following
    - a customisable title (default is taken from the youtube video title)
    - a youtube url 
    - a playback start time (default is start of video)
    - a playback end time (default is end of video)


# Technical requirements
- All user data must be stored in the client browser - no backend required 
- user settings should be stored and retrieved from browser storage on page load

# App Features
The app allows the user to enter youtube video urls to build a playlist
The user can create multiple playlists
    - A sidebar lists every playlist by title; clicking a title switches to that playlist
    - The user can create a new playlist via a "+ New playlist" button in the sidebar
    - The user can delete a playlist (only when more than one exists); deleting the active playlist switches to the next one
    - Switching playlists stops any in-progress jam (the queue belongs to the previous playlist)
The user can add custom playback options for each video
    - custom playback start and end points
    - custom playback speed
    - custom playback volume
If the app has not been used before, the user should start with a single default jam playlist that is empty
If the jam playlist is empty, an entry field for adding the youtube video url of the first jam video should exist
The "Let's Jam" button reads "Let's Jam to {title}" when the active playlist has been renamed; otherwise it reads just "Let's Jam"

# UI

The app uses a single combined view rather than discrete modes. Editing is inline: pressing **Edit** on a row swaps the row's metadata for an inline form; pressing **Edit** on the playlist title swaps the title heading for an inline form. There is no separate "Playback Mode" — the playlist remains visible and editable while a jam is in progress.

## Layout
- **Sidebar** (left) — list of playlists with active highlight, "+ New playlist", Loop-playlist toggle, Export / Import.
- **Main column** — H1, "Let's Jam" button, sticky player area (only while jamming), playlist title + Edit/Delete actions, add-URL form, and the list of jam videos.

## Playlist editing
- Click **Edit** next to the playlist title to rename it inline.
- Click **+ New playlist** to create a new empty playlist; it becomes active.
- Click **Delete** next to the playlist title to delete the active playlist (only available when more than one exists).
- Drag a video row's handle to reorder within the playlist, or drop it onto a sidebar entry to move it to that playlist. Keyboard equivalent: focus the handle, press **Space** to pick up, **↑ / ↓** to move within the list, **Space** again to drop, **Escape** to cancel.

## Video editing
- Click **Edit** on a video row to inline-edit its title, playback speed, volume, and start/end times.
- Click **Remove** to delete the video from the active playlist.
- Click **▶ Play** to start the jam at that video.

## Playback
- The **Let's Jam** button starts the jam from the first video in the active playlist.
- During a jam, a sticky player at the top of the main column shows the current video, current title, Prev / Next / Stop, and Speed / Volume controls (which write back to the active video's settings).
- When the playlist ends, playback stops unless **Loop playlist** is enabled in the sidebar settings.


### TEST DATA - SAFE TO IGNORE THE BELOW

### Sample youtube video urls to work with:
https://youtu.be/oDMldn7hFZo?si=_3Gl62JQf3EmsrTm
https://youtu.be/SRVE_-th1EI?si=zBzyIuQ6JXLzlW03
https://youtu.be/ydeV1_8pM4o?si=uViMOSxiVXordBAN
https://youtu.be/ootXJmTJl_c?si=VWfXOV_CNMq420H0

### Playlist data
{"title":"KGLW Jam","videos":[{"id":"1777786816194-7twu32","videoId":"ydeV1_8pM4o","url":"https://youtu.be/ydeV1_8pM4o?si=uViMOSxiVXordBAN","title":"King Gizzard & The Lizard Wizard - Ice V","addedAt":"2026-05-03T05:40:16.194Z","playbackRate":1,"volume":100,"startSeconds":9,"endSeconds":null},{"id":"1777786807841-g4plq3","videoId":"SRVE_-th1EI","url":"https://youtu.be/SRVE_-th1EI?si=zBzyIuQ6JXLzlW03","title":"King Gizzard & The Lizard Wizard - Gaia | Highway Holidays TV","addedAt":"2026-05-03T05:40:07.841Z","playbackRate":1,"volume":100,"startSeconds":31,"endSeconds":612},{"id":"1777786822699-2g2kkr","videoId":"ootXJmTJl_c","url":"https://youtu.be/ootXJmTJl_c?si=VWfXOV_CNMq420H0","title":"The Land Before Timeland","addedAt":"2026-05-03T05:40:22.699Z"},{"id":"1777786797085-4xb8mn","videoId":"oDMldn7hFZo","url":"https://youtu.be/oDMldn7hFZo?si=_3Gl62JQf3EmsrTm","title":"King Gizzard & The Lizard Wizard - The Dripping Tap (Audio)","addedAt":"2026-05-03T05:39:57.085Z"}]}
### Server
To serve file use: 
    python3 -m http.server 8080
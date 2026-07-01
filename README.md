# crate. A different way to experience your local Plex music collection.

A browsable "record crate" for your local Plex music library. Scroll through a shelf of generated album spines, tap one to see the cover and tracklist, and play straight through your browser — on your phone, your desktop, or a dedicated kiosk screen.

No app, no build step, no backend beyond Plex itself. It's a single self-contained HTML file.

## Features

* **Spine shelf** — every album renders as a generated "spine": a mix of album covers and colours
* **Plex sign-in** — standard Plex PIN-based OAuth flow, no API keys or manual tokens
* **Auto-reconnect** — checks whether the last-known server address still works and re-resolves via `plex.tv` if not
* **Artist shelves** — tap an artist's name to filter the shelf to just their albums
* **Real queue behavior** — browsing to a different album never interrupts what's playing. Tapping a track starts a new queue from that album. When an album finishes, the next one plays automatically — next release by the same artist chronologically, then the next artist alphabetically
* **Up Next panel** — see what's queued, jump to any upcoming track
* **Now-playing bar** — cover, title, artist, transport controls, progress, and a codec/bitrate badge (e.g. `FLAC 16-bit 44.1kHz`)
* **Tap-to-enlarge cover art**

## Requirements

* A Plex Media Server with a music library
* Plex Pass(?)
* Any modern browser (Safari, Chrome, Firefox)

## Getting started

Pick whichever hosting approach fits what you're doing with it — the app itself doesn't change.

### My Setup: Self-hosted on a Raspberry Pi Zero W

I decided I'd be using this at home mostly and would use various devices to play the albums. The best way was to use a Raspberry Pi I had lying around as the host for the .html file. Currently I'm using an iPad Mini 4th Gen to point to the RPi which is working great.

### Raspberry Pi Install:

On the Pi:

```bash
sudo apt update
sudo apt install nginx -y

```

On your computer (or wherever you've downloaded the source files):

```bash
scp crate-main.html <user>@<host>:/home/<user>/
ssh <user>@<host> 

sudo cp /home/<user>/crate-main.html /var/www/html/index.html
```

Visit `http://<host>.local/` from any device on the same network.

### Option 2: Kiosk display

Run the page in Chromium's `--kiosk` mode on a Pi (or similar SBC) connected to a touchscreen, pointed at `http://localhost/`. A quad-core board with hardware-accelerated Chromium (Raspberry Pi 4 or newer, or comparable) is recommended — Chromium needs at least 1GB RAM and doesn't run well on single-core boards.

### Option 3: Add to home screen / Dock

* **iOS/Android**: open the hosted URL in the browser, then *Add to Home Screen*
* **macOS (Safari)**: open the URL, then *File → Add to Dock*

## Customization

Everything lives in one file, so most tweaks are a single CSS variable or constant away:

* **Color palette** — CSS custom properties at the top of `<style>` (`--bg`, `--accent`, `--wood`, etc.)
* **Spine artwork size** — `SPINE\_CANVAS\_W` / `SPINE\_CANVAS\_H` constants in the script
* **Fonts** — swapped via the Google Fonts `<link>` in `<head>` (Fraunces for display type, Inter for UI, JetBrains Mono for numeric/technical text)

## Known limitations

* **Spine artwork requires CORS-enabled image reads** from your Plex server. If your server/proxy doesn't allow cross-origin pixel reads, spines fall back to a plain color placeholder with a text label instead of the generated artwork — playback and browsing still work fine either way.
* Data (auth token, server address, library selection) is stored in the browser's `localStorage`, so it's per-browser, not synced across devices.

## Note: Created alongside Claude - be aware parts of this project are vibe coded!



# Main cover view:
![alt text](https://github.com/tommfy/crate-plex-album-flow/blob/main/Readme%20Resources/main_cover_view.png "Main cover view")


# Main cover view when something is playing:
![alt text](https://github.com/tommfy/crate-plex-album-flow/blob/main/Readme%20Resources/main_view_playing.png "Main cover view when something is playing")


# Now playing queue - what's up next:
![alt text](https://github.com/tommfy/crate-plex-album-flow/blob/main/Readme%20Resources/now_playing_queue.png "Now playing queue - what's up next")


# Clicking on an album with bring up its tracks:
![alt text](https://github.com/tommfy/crate-plex-album-flow/blob/main/Readme%20Resources/album_tracklisting.png "Clicking on an album with bring up its tracks")


# Clicking on an artist name in the album view will show all albums by that artist in their own shelf:
![alt text](https://github.com/tommfy/crate-plex-album-flow/blob/main/Readme%20Resources/artist_shelf.png "Clicking on an artist name in the album view will show all albums by that artist in their own shelf")

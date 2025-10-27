# PharmApp — Subtitle Media Fetcher + Player

A tiny, cross‑platform Tkinter app to **download MP3/MP4 next to .vtt subtitles** using `yt-dlp`, and to **play media with bilingual subtitles (EN/VI)**. Designed with a compact **PharmApp theme**.

> Current release: **v3.1**  
> OS: Windows / macOS / Linux

---

## ✨ Features

- **Download tab**
  - Scan your root folder for `.vtt` files (recursively).
  - **Download MP3/MP4** next to the subtitle file (same name; skip if exists).
  - Built‑in anti‑403: uses `youtube:player_client=android` and CLI fallback `--cookies-from-browser` (Chrome/Edge/Firefox/Brave).

- **Player tab**
  - Play **MP3/MP4** with **bilingual subtitles overlay** (EN on top, VI below) synced to playback.
  - Uses `python-vlc` + VLC runtime, embedded player window.
  - “Download Missing” button if media is absent.

- **Subtitle Search → URL tab**
  - Search subtitles by title or `[YouTubeID]`.
  - Generate a **list of YouTube URLs**, copy to clipboard, or **export to `url_yt.txt`** anywhere you like.

- **Help tab** + **single‑line footer** with quick links and **Donate** button.
- Works with long folder trees and Unicode filenames.
- Compact UI that saves vertical space.

---

## 📦 Project Layout

Single‑file app (you can rename freely):

```
Subtitle_Media_Fetcher_v3.py
nct_logo.png        # window icon (PNG)
nct_logo.ico        # Windows .exe icon
cookies.txt         # optional: exported browser cookies (for yt-dlp)
```

Your subtitle files can live anywhere (the app scans recursively). **Recommended naming** (so MP3/MP4 sit next to the subtitle):

```
Bài học nghiêm khắc từ Covid-19 [V4D3zeW_slg] - 2021-02-01.vi.vtt
Bài học nghiêm khắc từ Covid-19 [V4D3zeW_slg] - 2021-02-01.en.vtt
# After download (auto):
Bài học nghiêm khắc từ Covid-19 [V4D3zeW_slg] - 2021-02-01.vi.mp3
Bài học nghiêm khắc từ Covid-19 [V4D3zeW_slg] - 2021-02-01.vi.mp4
```

The **YouTube ID** is extracted from the filename pattern: `[...]` (e.g. `[V4D3zeW_slg]`).

---

## 🚀 Quickstart

```bash
# 1) Install Python deps
pip install yt-dlp python-vlc

# 2) Install ffmpeg
#   Windows: winget install Gyan.FFmpeg
#   macOS:   brew install ffmpeg
#   Linux:   sudo apt-get install ffmpeg

# 3) Install VLC (runtime for python-vlc)
#   Windows/macOS: videolan.org  |  Linux: sudo apt install vlc

# 4) Run
python Subtitle_Media_Fetcher_v3.py
```

> Tip: Place `cookies.txt` (exported from your browser) next to the script to improve success rate; the app also **falls back** to `--cookies-from-browser` via `yt-dlp` CLI when needed.

---

## 🧩 Usage

### Download tab
1. **Root**: pick a folder to scan for `.vtt` (recursive).  
2. **Search**: filter the list quickly.  
3. Select one or multiple `.vtt` → click **MP3**, **MP4**, or **Both**.  
4. Files are saved **next to the subtitle** and **skipped** if they already exist.

### Player tab
- Share the same root or pick another.  
- Search and select an item (grouped by `[YouTubeID]`).  
- **Play MP3** or **Play MP4**: bilingual overlay (EN above, VI below), synced to playback.  
- If media is missing, use **Download Missing**.

> VLC can only display a single external subtitle at a time; this app implements a **two‑line overlay in the UI**, so you always see both languages.

### Subtitle Search → URL tab
- Search by title or ID, the view is **de‑duplicated by ID**.
- **Copy URLs** (selected/all) to clipboard, or **Export** to a `url_yt.txt` file anywhere.

---

## ⌨️ Keyboard Shortcuts (Download tab)

- **Ctrl+O** – Browse root  
- **Ctrl+F** – Focus search  
- **F5** – Rescan  
- **Ctrl+1** – Download MP3  
- **Ctrl+2** – Download MP4  
- **Ctrl+D** – Download Both  
- **Ctrl+E** – Open folder of selected file  
- **Ctrl+Q** – Quit app

---

## 🛠️ Build Binaries

> Requires `pyinstaller`:
```bash
pip install pyinstaller
```

### Windows (.exe)
```bash
pyinstaller --onefile --windowed --icon nct_logo.ico --add-data "nct_logo.png;." Subtitle_Media_Fetcher_v3.py
# Output: dist/Subtitle_Media_Fetcher_v3.exe
# Tip: put ffmpeg.exe next to the .exe if user PATH lacks ffmpeg
```

### macOS (.app)
```bash
pyinstaller --onefile --windowed --add-data "nct_logo.png:." Subtitle_Media_Fetcher_v3.py
# If you have an .icns icon:
# pyinstaller --onefile --windowed --icon nct_logo.icns --add-data "nct_logo.png:." Subtitle_Media_Fetcher_v3.py
# Output: dist/Subtitle_Media_Fetcher_v3.app
```

> The `--add-data` syntax differs by OS: `;` on Windows, `:` on macOS/Linux.

---

## 🔒 Handling 403 / Forbidden

The app uses a **ladder** of strategies to reduce 403s:
1. `yt-dlp` Python API with `extractor_args="youtube:player_client=android/web"`  
2. **Retry** with more generic formats  
3. **CLI fallback** with `--cookies-from-browser` (Chrome/Edge/Firefox/Brave)  
4. Finally, **no‑cookies** CLI attempt

You can also place a `cookies.txt` (Netscape format) next to the script for an extra option.

---

## 🧠 How subtitles are parsed

- Supports both `hh:mm:ss.mmm` and `mm:ss.mmm` VTT timestamps.  
- Minimal parser removes simple HTML tags inside cues.  
- App overlays **EN (top)** and **VI (bottom)** lines in real time using `python-vlc` playback time.

---

## 🙋 Troubleshooting

- **VLC missing**: install VLC runtime and `python-vlc` (`pip install python-vlc`).  
- **No embedded video on macOS**: some macOS/VLC builds may open a separate window; playback still works.  
- **Unicode filenames**: supported; ensure your terminal/code page can display them if running from CLI.  
- **403 Forbidden**: see the section above; keep `yt-dlp` up to date (`pip install -U yt-dlp`).  
- **ffmpeg not found**: install as shown above or place `ffmpeg(.exe)` next to the app/binary.

---

## 🗺️ Roadmap

- [ ] Seekbar + playback speed (0.75× / 1.0× / 1.25×)  
- [ ] Batch export URL lists by folder  
- [ ] Optional SRT export from VTT

Contributions & PRs are welcome!

---

## 🧾 License

MIT License. See `LICENSE` (you can change the license as you prefer for your repo).

---

## ❤️ Credits

- **yt-dlp** for robust media extraction  
- **VLC / python-vlc** for cross‑platform playback  
- **ffmpeg** for audio extraction and muxing

**PharmApp Theme** — by Nghiên Cứu Thuốc / PharmApp.  
Discover • Design • Optimize • Create • Deliver.


# Vox Design: Vox-Style Documentary & Explainer Engine

[![Platform](https://img.shields.io/badge/Platform-Antigravity%20%7C%20Claude%20Code-4285F4)](https://github.com/nirajan-khatiwada/VOX-DESIGN)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Audio Standard](https://img.shields.io/badge/Audio-EBU%20R128%20%28--14%20LUFS%29-orange.svg)](#sound-design--audio-engineering)

An end-to-end production framework and AI agent skill system for creating authentic **Vox-style documentary explainers** on any topic. 

Built natively for **Google Antigravity** (with backwards compatibility for Claude Code), it guides you from scriptwriting and voiceover alignment to bespoke cutout sticker generation, code-drawn data animations, maps with coordinate pins, layered sound design, and master MP4 rendering.

---

## 📑 Table of Contents

- [Core Philosophy](#-core-philosophy)
- [Architecture & Skills](#-architecture--skills)
- [The 9-Stage Production Pipeline](#-the-9-stage-production-pipeline)
- [Visual Design: With & Without Flow](#-visual-design-with--without-flow)
- [Sound Design & Audio Engineering](#-sound-design--audio-engineering)
- [Installation & Quick Start](#-installation--quick-start)
- [Directory Structure](#-directory-structure)
- [License & Commercial Rights](#-license--commercial-rights)

---

## 🎨 Core Philosophy

Most explainer videos fail because visuals are treated as generic background decoration. This engine enforces the proven principles of Vox editorial visual journalism:

1. **Plan Scenes from Narration, Never Shot Lists**: Visuals enter on the *exact word* that names them (word-level sync via Whisper).
2. **Every Spoken Noun Gets a Bespoke Cutout**: No repeating generic icons. "Victoria Terminus" shows the Gothic colonial dome; "Tea garden" shows a worker harvesting tea.
3. **Data & Clocks Drawn in Code (Never AI)**: AI hallucinates clock hands, charts, and bar heights. The engine draws clock sweeps (`6:00 → 7:00`), timeline rulers, and counters programmatically with Pillow and FFmpeg.
4. **Editorial Color Palette**:
   - **Background**: Warm cream paper texture (`RGB(241, 233, 216)`) with subtle grain; dark navy (`RGB(26, 33, 58)`) for night/serious acts.
   - **Ink & Accents**: Warm charcoal ink (`RGB(42, 36, 32)`), Vox Red (`RGB(217, 69, 43)`), Highlighter Yellow (`RGB(242, 183, 5)`).
5. **Labels on Everything**: Subtitle/caption overlays (`LBL`), highlight boxes (`KEY`), dark tags (`TAG`), and massive statistic figures (`NUM`) burned with complex script shaping (`libass`).

---

## 🧩 Architecture & Skills

The engine ships as two companion skills:

### 1. `vox-doc`
The master director skill managing:
- Scriptwriting (`SCRIPT.md` and `LINES.tsv`) with factual hook-twist pacing.
- Word-level alignment (`WORDS.tsv`) using `faster-whisper`.
- Scene orchestration (`SCENES.tsv` and `ELEMENTS.tsv`).
- Automated shot rendering and master compilation with `render.py`.

### 2. `vox-doc-sound`
The sound design and audio engineering engine:
- Bundles **1,000+ free commercial-use SFX** (`Stable Audio Open` + `Kenney CC0`).
- Bundles **22 mood-based instrumental music beds & ambient pads** generated with Google Gemini Lyria.
- Implements word-level cue placement (`cues.tsv`), ducking music under voice and sound effects, and automated EBU R128 loudness mastering (-14 LUFS integrated, -1.5 dBTP).

---

## 🎬 The 9-Stage Production Pipeline

```
SCRIPT.md ──► LINES.tsv ──► VO.wav ──► align.py ──► WORDS.tsv
                                                         │
SCENES.tsv ◄─────────────────────────────────────────────┘
    │  ELEMENTS.tsv (1 bespoke PNG per spoken noun)
    │       │
    │   elements.py (Chroma-key magenta) ──► key.py ──► elements/cut/*.png
    ▼
render.py shots ──► shots/NN.mp4 + labels.ass (PIL frames + code graphics)
render.py master ──► Film with labels (ass:shaping=complex) + mix.wav ──► FINAL.mp4
```

| # | Stage | Description | Artifact |
|---|---|---|---|
| **1** | **Script** | 15 spoken lines/minute. Structure: Hook → Facts → History → Cost/Problem → Global context → Question → Twist → Outro. | `SCRIPT.md`, `LINES.tsv` |
| **2** | **Voice** | User recording, ElevenLabs, Kokoro (English), or Indic Parler on Kaggle (Hindi/Indian languages). | `VO.wav`, `VO.tsv` |
| **3** | **Word Timing** | Word-level timestamp extraction using local `faster-whisper`. | `WORDS.tsv` |
| **4** | **Scene Plan** | Mapping visual elements (PNGs, clocks, maps, tags) to specific spoken words. | `SCENES.tsv`, `ELEMENTS.tsv` |
| **5** | **Pictures** | Generating isolated objects on pure magenta (`#FF00FF`) and keying them into transparent PNGs. | `elements/cut/*.png` |
| **6** | **Maps** | Measuring coordinate pins (`u, v` 0–1) on flat map silhouettes. | `PLACES.tsv` |
| **7** | **Shots** | Rendering PIL animation frames at 24fps (1080p) into individual scene MP4s. | `shots/*.mp4` |
| **8** | **Sound** | Placing whooshes on act transitions, paper sounds on stickers, and ducked music beds. | `cues.tsv`, `mix.wav` |
| **9** | **Master** | Burning ASS subtitles with complex script shaping (`ass=labels.ass:shaping=complex`) and muxing audio. | `<project>.mp4` |

---

## 🖼 Visual Design: With & Without Flow

### Option A: Using Google Flow (Nano Banana 2)
The skill includes automated batch submission scripts for Google Flow:
- Uses `flow-auto` CLI with pacing (`--batch 8 --delay 5-8`).
- Generates on `#FF00FF` flat magenta.
- [key.py](file:///c:/Users/Nirajan/Downloads/lastone/vox-doc/skills/vox-doc/scripts/key.py) unmixes edge colors to preserve crisp dark outlines without color fringing.

### Option B: Creating the Vox Aesthetic WITHOUT Flow
The pipeline only requires transparent PNGs in `elements/cut/`. You can use any of these alternatives:

1. **Archival Scans + Background Removal (The Real Documentary Method)**:
   - Source historical photos or diagrams from Wikimedia Commons, US National Archives, or Flickr Commons.
   - Run `rembg` locally to strip the background:
     ```bash
     python -m pip install rembg
     python -c "from rembg import remove; from PIL import Image; remove(Image.open('archival.jpg')).save('elements/cut/01.png')"
     ```
2. **Any AI Generator (Midjourney, DALL-E 3, Flux, Recraft)**:
   Use the tested prompt:
   > `[Subject], bold flat vector illustration with a thin dark outline, Vox editorial explainer style, warm muted vintage colours, one single isolated object, centred with a generous margin of empty space on all sides, on a solid flat pure magenta background #FF00FF, hard crisp edges, no shadow, no text, no background scenery`
   Run `python skills/vox-doc/scripts/key.py <folder>` to strip the magenta.
3. **Editorial Vectors (Recraft.ai / Noun Project / Freepik)**:
   - Select "Vector Illustration" or "Editorial" style.
   - Export directly as transparent PNGs.

---

## 🔊 Sound Design & Audio Engineering

Vox explainers maintain strict audio restraint:
- **No trailer hits or booms**: No braams, dramatic risers, or meme sound effects.
- **Act Whooshes**: Soft whooshes occur only on chapter/act transitions (3–5 per video).
- **Stickers & Pins**: Paper card-placements (`card-place-1.ogg`) or pin-drops at `-25 dB`.
- **Music Ducking**: The background bed sits ~20 dB under the voice (`-23 dB`) and ducks automatically under sound effects and narration.
- **Master Target**: Integrated loudness of `-14 LUFS` and true peak `<= -1.5 dBTP`.

Verify loudness via FFmpeg:
```bash
ffmpeg -i mix.wav -af ebur128=peak=true -f null -
```

---

## 🚀 Installation & Quick Start

### 1. Prerequisites
- **Python 3.11–3.13** (3.12 recommended)
- **FFmpeg with `libass`** (Full build)
  - Windows: `winget install Gyan.FFmpeg`
  - macOS: `brew install ffmpeg`
  - Linux: `sudo apt install ffmpeg`
- **Python Libraries**:
  ```bash
  python -m pip install numpy pillow scipy soundfile faster-whisper
  ```

### 2. Global Antigravity Installation
Clone or copy this repository:
```bash
# Windows PowerShell
Copy-Item -Path "skills\vox-doc" -Destination "$HOME\.gemini\config\skills\vox-doc" -Recurse -Force
Copy-Item -Path "skills\vox-doc-sound" -Destination "$HOME\.gemini\config\skills\vox-doc-sound" -Recurse -Force
```

### 3. Start Creating
In any Antigravity conversation, simply type:
- `"vox doc start"` — Begin a new documentary.
- `"vox doc continue"` — Resume a project from its last checkpoint.
- Or provide a topic: `"Create a Vox doc about the history of the Suez Canal"`.

---

## 📁 Directory Structure

```text
VOX-DESIGN/
├── GEMINI.md                    # Antigravity project rules & entrypoint
├── AGENTS.md                    # Multi-agent standard instructions
├── CLAUDE.md                    # Claude Code compatibility instructions
├── START_HERE.txt               # User-friendly quickstart guide (EN / Hinglish)
├── skills/
│   ├── vox-doc/                 # Master explainer video skill
│   │   ├── SKILL.md             # Antigravity skill instructions
│   │   ├── SETUP.md             # One-time tool setup & verification
│   │   ├── PLAYBOOK.md          # Complete visual storytelling methodology
│   │   ├── example/             # Tested reference project (Indian Standard Time)
│   │   │   ├── SCENES.tsv
│   │   │   ├── ELEMENTS.tsv
│   │   │   ├── PLACES.tsv
│   │   │   └── VO.tsv
│   │   └── scripts/
│   │       ├── align.py         # Whisper word-level speech alignment
│   │       ├── elements.py      # Batch Flow generator
│   │       ├── key.py           # Magenta chroma-key unmixer
│   │       ├── cutout.py        # Alpha edge refine & cropping
│   │       └── render.py        # Frame renderer, clock animator, ASS burner
│   └── vox-doc-sound/           # Sound design & mixing skill
│       ├── SKILL.md             # Audio rules and cue sheet syntax
│       ├── README.md            # Sound kit guide
│       ├── scripts/
│       │   ├── mix.py           # Ducking & mastering engine (-14 LUFS)
│       │   ├── index_sfx.py     # SFX & music catalog builder
│       │   ├── vo_join.py       # Narration joiner with gap spacing
│       │   ├── kokoro_vo.py     # Local CPU speech generation
│       │   └── eleven_sfx.py    # Optional ElevenLabs integration
│       ├── sfx/                 # 1,000+ bundled SFX (Stable Audio + Kenney CC0)
│       └── music/               # 22 Gemini Lyria instrumental beds
└── README.md
```

---

## ⚖️ License & Commercial Rights

- **Code & Scripts**: Released under the [MIT License](LICENSE).
- **Sound Library (`sfx/StableAudio/`)**: Free for commercial use and sharing under the Stable Audio Open license.
- **Kenney Assets (`sfx/Kenney_CC0/`)**: Public Domain (CC0 1.0).
- **Bundled Music (`music/Gemini/`)**: Instrumental beds generated with Google Gemini Lyria; free for commercial use and sharing.
- **Your Videos**: Any explainer video you render using this engine is 100% yours to monetize and publish.

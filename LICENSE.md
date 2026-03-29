# License

## Repository Code

The automation code, workflows, and documentation in this repository are licensed under the MIT License.

```text
MIT License

Copyright (c) 2024-2026 MeedyaDL

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
```

---

## Bundled Third-Party Tools

The binaries distributed via this repository's GitHub Releases are redistributed under their respective upstream licenses. **This repository does not modify any of these tools** — it only provides packaging automation and standardized naming.

| Tool | Upstream Project | License | Source |
| ---- | --------------- | ------- | ------ |
| **FFmpeg** | FFmpeg | GPL v2+ / LGPL v2.1+ | [BtbN/FFmpeg-Builds](https://github.com/BtbN/FFmpeg-Builds) |
| **yt-dlp** | yt-dlp | Unlicense | [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) |
| **mp4decrypt** | Bento4 | GPL v2 | [axiomatic-systems/Bento4](https://github.com/axiomatic-systems/Bento4) |
| **MP4Box** | GPAC | LGPL v2.1 | [gpac/gpac](https://github.com/gpac/gpac) |
| **aria2c** | aria2 | GPL v2 | [aria2/aria2](https://github.com/aria2/aria2) |
| **fpcalc** | Chromaprint | LGPL v2.1 | [acoustid/chromaprint](https://github.com/acoustid/chromaprint) |
| **N_m3u8DL-RE** | N_m3u8DL-RE | MIT | [nilaoda/N_m3u8DL-RE](https://github.com/nilaoda/N_m3u8DL-RE) |
| **get_iplayer** | get_iplayer | GPL v3 | [get-iplayer/get_iplayer](https://github.com/get-iplayer/get_iplayer) |
| **Votify** | Votify | No explicit license | [glomatico/votify](https://github.com/glomatico/votify) |
| **gytmdl** | gytmdl | No explicit license | [glomatico/gytmdl](https://github.com/glomatico/gytmdl) |
| **gamdl** | gamdl | No explicit license | [glomatico/gamdl](https://github.com/glomatico/gamdl) |
| **AMdecrypt** | amdecrypt | No explicit license | [glomatico/amdecrypt](https://github.com/glomatico/amdecrypt) |
| **Wrapper** | wrapper | No explicit license | [WorldObservationLog/wrapper](https://github.com/WorldObservationLog/wrapper) |
| **MediaInfo** | MediaInfo | BSD-2-Clause | [MediaArea/MediaInfo](https://github.com/MediaArea/MediaInfo) |
| **OF-Scraper** | OF-Scraper | MIT | [datawhores/OF-Scraper](https://github.com/datawhores/OF-Scraper) |
| **MKVToolNix** | MKVToolNix | GPL v2 | [mkvtoolnix.download](https://mkvtoolnix.download/) |

### License Compliance Notes

- **GPL v2+ tools** (FFmpeg, mp4decrypt, aria2c, MKVToolNix): Source code is available at the upstream repositories linked above. These binaries are unmodified redistributions.
- **LGPL v2.1 tools** (MP4Box, fpcalc): Dynamically linked where applicable. Source code available at upstream repositories.
- **BSD-2-Clause tools** (MediaInfo): Very permissive; redistribution requires retaining the copyright notice and disclaimer.
- **Unlicensed tools** (yt-dlp): Distributed under the Unlicense, placing the work in the public domain.
- **No explicit license tools** (Votify, gytmdl, gamdl, AMdecrypt, Wrapper): These tools are publicly available on GitHub but do not include an explicit license. They are redistributed here in good faith for interoperability with MeedyaDL. If any upstream author objects to redistribution, the tool will be promptly removed.

### Your Obligations

If you redistribute MeedyaDL or any of the bundled tools:

- You must comply with each tool's respective license
- For GPL/LGPL tools, you must make the corresponding source code available or provide a written offer to do so
- The upstream source links in the table above satisfy this requirement for unmodified binaries

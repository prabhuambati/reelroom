# Collection workflow

Use only scoped public sources and files you are permitted to process. Public availability, attribution, personal storage and non-commercial use do not themselves establish media rights. Do not bypass private accounts, access challenges, login restrictions or platform controls. This guide does not grant permission to any creator's recordings.

## Browser-assisted metadata capture

1. In a Chromium-compatible browser, open the extension management page, enable developer mode and load the repository's `extension/` folder unpacked.
2. Review the requested permissions. The extension declares activeTab, downloads, storage and scripting; its explicit host permission is restricted to `https://www.instagram.com/*`.
3. Navigate yourself to an authorized public profile, hashtag or search source. You may use your own existing login session, but the extension does not ask for credentials.
4. Use **Collect visible reels** or the optional current-source scrolling scan. The 500-record per-source cap is an upper bound, not a promise of completeness. Changing Instagram layouts, unloaded cards or protected pages may prevent capture.
5. Download JSON. Sign into the local Reelroom Admin console and import it there. The extension does not bypass CSRF by posting directly to the server.

Metadata may include links, uploader/profile fields, displayed audio labels and other visible source evidence. Public location text must be explicit and current; do not infer a person's home state from language, tags, venue or older biographies. The location parser is not a verified-location crawler or a regional analytics product.

## Review and analysis

Import normalizes reel URLs and deduplicates source records. Heuristics can screen captions/tags, but text and studio/background audio are not proof of a human performance. Inspect actual permitted evidence, distinguish singing/playing from lip-sync, narration, promotions or reuploads, and keep uncertain cases in review. Admin acceptance is a collection decision, not copyright clearance.

Optional local media analysis:

```bash
python run.py analyze-video /path/to/permitted-video.mp4 --caption "Explicit source context"
```

No-key mode uses the deterministic heuristic fallback. Setting `OPENAI_API_KEY` can enable selected frame/audio model analysis and may transmit material to the provider and incur charges. Model availability, model names and output schemas can change. Validate results manually; current classification is not production-grade or proof of live performance.

## Local conversion

Only use files you already have permission to process:

```bash
python run.py process-video /path/to/permitted-video.mp4 \
  --reel-url "https://www.instagram.com/reel/EXAMPLE_ID/" \
  --username example_creator
```

The URL/username above are fictional placeholders, not a supplied source or rights grant. FFmpeg is discovered from the system, the installed `imageio-ffmpeg` package, or the legacy local vendor fallback. Conversion writes attributed MP3 metadata and records the source/file hashes. Review resulting status in Admin; a converted file is not automatically a verified or permitted public release.

This repository does not include an automatic exact-source download API, a media catalog or a guarantee that arbitrary reels can be retrieved. Existing review decisions and source matches must be respected rather than replaced by guessed media.

## Provenance

Keep the original reel/profile link, uploader and explicitly captured coauthor references. Do not present listed coauthors as verified singers/instrumentalists without evidence. Caption titles and displayed audio labels are distinct from acoustic identification. Leave ambiguous titles unidentified. Missing MP3s and awaiting-review records are different states; neither should be presented as playable.

Export files and model evidence remain local under ignored runtime paths. Inspect any file before sharing it: exports can contain creator/source metadata and local paths.

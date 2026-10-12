# Reelroom

### Small stages. Big feelings.

An authenticated, local-first performance library built around public Instagram reel metadata and permitted local audio. Discover independent voices, preserve original creator credits, review uncertain performances, and listen through a music-app interface that feels like your own space.

**Status:** tested local MVP · **Python 3.10+** · **SQLite** · **Vanilla JavaScript** · **82 automated tests**

> This repository contains source code only. It does **not** include a music catalog, downloaded reels, MP3s, creator records, screenshots, databases, accounts, passwords, API keys or browser sessions. A fresh installation starts empty. Public access and creator credits do not establish permission to redistribute media.

## What Reelroom does

| Area | Available today |
|---|---|
| Listening | Search, creator browsing, artwork-led shelves, a now-playing panel, playback, seeking, previous/next, natural queue advance and volume |
| Two layouts | Cards and compact numbered List rows; highlighted accessible toggle; Home defaults to Cards, with List still selectable; other sections remember the browser preference |
| Personal library | Account-private playlists, playlist ordering and favorites |
| Authentication | Local username/password signup, owner-only first-admin bootstrap, expiring sessions, logout and backend authorization |
| Creator access | Manual Instagram verification followed by a short-lived, single-use, account-bound invitation; scoped correction/removal requests |
| Review | Admin-only import/review/export console; accepted, awaiting-review, rejected and missing-media states |
| Collection | Browser extension captures visible reel metadata; optional current-source scrolling, URL normalization and duplicate detection |
| Attribution | Original reel/profile links, conservative caption-title provenance, uploader/listed-coauthor credits where captured, honest artwork placeholders |
| Analysis | Text heuristics; optional visual/audio model assistance when explicitly configured; uncertain cases remain review candidates |
| Local media | FFmpeg conversion for files you are permitted to process; authenticated HTTP byte-range serving for available accepted MP3s |

Reelroom is not Spotify, an official Instagram integration, a production streaming service or an automatic copyright-clearance system. The home shelves use the local collection—not fabricated recommendations or popularity rankings.

## Quick start

### 1. Clone and install

```bash
git clone https://github.com/prabhuambati/reelroom.git
cd reelroom
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

On Windows, activate with `.venv\Scripts\activate` instead. The executable `.command` helper is macOS-specific; the Python CLI works directly on supported platforms. Tests use standard-library `unittest`.

### 2. Start the local server

```bash
python run.py serve
```

Open **http://127.0.0.1:8765**. The server binds to loopback and refuses non-loopback hosts. Do not expose this build to your network or the internet.

### 3. Create the first admin yourself

In a second terminal, activate the same environment, then run:

```bash
python run.py setup-code
```

Open **http://127.0.0.1:8765/setup**. Enter the one-use code printed in your terminal and choose your own username and password. The code expires after 15 minutes. There is no default admin account or password.

- Username: 3–32 lowercase letters, numbers or underscores.
- Password: 12–256 characters; uppercase letters and symbols are allowed.
- Keep the code and credentials private. Never paste them into issues or chat.
- After owner setup, public signup creates **listener accounts only**.

macOS users can also run `bash scripts/setup-admin.command` after activating the environment.

### 4. Add your own permitted collection

A fresh library has no tracks. Import metadata through the authenticated Admin console, review records, and process only local files you are allowed to use. See [the collection workflow](docs/collecting.md). Model analysis is optional; login and listening do not require an API key.

## How it is organized

```text
Public-source browser metadata / permitted local files
                    │
              Admin import + review
                    │
       collection.db + local media folder
                    │
        Authenticated listening interface
                    │
     accounts.db: sessions, private playlists,
     favorites, invitations and creator bindings
```

```text
collector/
  core.py                 Collection, normalization, review, exports, conversion
  auth.py                 Accounts, sessions, private data, creator invitations
  library.py              Evidence-backed display metadata and media availability
  location.py             Conservative public-location parsing
  model_classifier.py     Optional model-assisted performance screening
  web/                    Login, listening UI, admin console and layout previews
extension/                Browser-assisted public metadata capture
scripts/                  Owner-run bootstrap helper
tests/                    Isolated unit and HTTP security regressions
docs/                     Architecture, workflow, security and validation
run.py                    Local server and CLI
```

## Permissions and creator verification

**Listeners** can access available accepted performances and their own playlists/favorites. They cannot access admin reviews, imports, exports, private source URLs or another user's personal data.

**Creators** do not gain ownership by signing up. An admin must manually verify a DM from the claimed Instagram account to the configured official contact, confirm the intended existing Reelroom account, and record a minimal evidence reference. The invitation is bound to that account and creator page; it is single-use, expires in 24 hours and is delivered manually through the verified conversation. No Instagram credentials are requested, and no messages or emails are sent automatically.

Creator access permits correction/removal **requests**, not direct collection edits, cross-creator changes, automatic downloads or redistribution rights. The official contact is unset on a fresh installation, so invitation issuance stays disabled until the owner configures it. Recorded identity conflicts and competing ownership claims require manual review.

**Admins** use the protected review/import/export console and manage manual invitations. There is no public admin signup. Legacy shared playlists remain preserved rather than assigned silently to a new account.

Read [architecture and permissions](docs/architecture.md) and [security boundaries](docs/security.md).

## Evidence, not invented metadata

Caption-derived song names are labeled **not audio-verified**. Ambiguous names stay **Unidentified performance**. Captured reel frames are labeled as captured images—not official cover art or guaranteed profile photos. Credits distinguish uploader/listed coauthors from verified performance roles.

List duration values are shown for a loaded track; unprobed tracks show a dash. This release does not persist duration metadata for every file. Regional parsing exists, but a state label must come from explicit public evidence; there are no validated regional rankings or fabricated analytics.

## Run the tests

```bash
python -m unittest discover -s tests -q
```

**82 automated tests passed** in the clean source export. Coverage includes normalization, review/display behavior, privacy isolation, password/session handling, owner bootstrap, invitation expiry/replay/revocation/concurrency, CSRF/origin/Host checks, backend authorization, private media access, HTTP ranges and packaging regressions.

Browser QA covered the redesigned desktop interface, 390px/360px frames, login/signup, private-data isolation, both layouts, playback/seeking and natural track advance. Home-layout policy also received a focused JavaScript state check. These are not real-device iOS/Android or full-duration all-track tests. Details: [validation](docs/validation.md).

## Local data and publication safety

Runtime data belongs under `data/`, which is ignored by Git. Keep databases, media, exports, source captures, account/session records and private keys out of commits. Environment files and the local FFmpeg/vendor bundle are also excluded. Sample password strings in tests are disposable fixtures, not operational credentials or default accounts.

The optional analyzer reads `OPENAI_API_KEY` from the environment. Configuring it may send selected frames/audio to the provider and incur costs; do not configure it for material you are not authorized to share. No live key is bundled or required for the default no-key workflow.

## Known limits and roadmap

- Local MVP, not production authentication or hosting: HTTP cookies are a loopback-only development exception.
- No Google login, passkeys, email verification/delivery or password-reset service.
- Stronger admin authentication, HTTPS/Secure cookies, CSP hardening, recovery, monitoring and a security review are needed before public hosting.
- No real-device background/lock-screen playback validation yet.
- Browser metadata collection depends on changing Instagram UI and available public pages; it is not an exhaustive crawler.
- Exact-source retrieval is not a guaranteed automatic API. No bypass of private accounts, challenges or access restrictions is provided.
- Rights, takedown handling and retention policies remain owner responsibilities.

## Project lineage and licensing

Reelroom builds on the earlier [Instagram Reel Collector prototype](https://github.com/prabhuambati/instagram-reel-collector). That repository is left unchanged by this publication.

No license has been selected for this repository. Public visibility does not mean public-domain code or unrestricted media rights.

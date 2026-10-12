# Validation

The source-only publication was prepared on 2026-10-12. It starts with an empty collection; private media, captured records, account/session data, source evidence, machine paths and screenshots were not included.

## Automated checks

```bash
python -m unittest discover -s tests -q
```

82 tests passed in the clean publication folder, including:

- Import/URL normalization and source/media deduplication.
- Conservative title, artwork, availability and location behavior.
- Salted password hashes, minimum password length, one-use/expiring admin bootstrap.
- Listener-only signup, login/logout, session expiry and auth rate limiting.
- Playlist/favorite account isolation and unavailable/invalid track rejection.
- Manual verification/contact requirements; account-bound invite tampering/expiry/replay/resend/revocation; concurrent redemption with exactly one winner.
- Creator request ownership boundaries and competing-owner protection.
- Anonymous/admin/role gates, sanitized listener responses, CSRF/origin/Host checks.
- Authenticated media ranges, traversal denial and blocking of non-playback source files.
- Declared extension scripting permission and ordinary installed-package FFmpeg discovery.

JavaScript syntax checks passed for the listening script, extension scripts and inline login/admin scripts. A focused state check validated Home's default Cards behavior, optional Home List selection, Home re-entry reset and independent saved preferences elsewhere.

## Browser checks from the local MVP

The authenticated redesign was checked using disposable isolated QA accounts, not production credentials. Login/signup forms, private playlist/favorite isolation and listener denial of admin APIs were exercised. Desktop and 390px/360px browser-frame layouts fit without horizontal overflow. Playback, seeking, previous/next, pause/resume and mobile source-credit access passed; natural track advance was observed from a real ended event after seeking near the end of a track.

Cards/List switching preserved the active audio source and progressing playback time. Compact List showed the full available local set once, search returned matching performances, both phone toggles were at least 44px tall, and list preference survived a reload. A later browser check verified Home defaulted to Cards and restored Cards when revisited, without changing the saved preference used by other sections.

Private collection rows, existing MP3 hashes and legacy playlists matched their pre-change snapshot after the UI/security milestones. These runtime audit files remain private and are not part of this repository.

## What this does not prove

No exhaustive classifier accuracy benchmark, complete fresh live-Instagram extension scan, third-party security audit, production-hosting validation, real-device iOS/Android test, lock-screen/background playback test, email verification/recovery test or full-duration listening test of every track was performed for this publication. The extension permission and installed-package FFmpeg fixes received regression checks, not a new live external collection run.

A local test server process ended during an earlier browser session; affected artwork/API checks were investigated and repeated after relaunch. Interrupted checks were not counted as passes.

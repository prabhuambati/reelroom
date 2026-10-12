# Architecture

## Components

`run.py` exposes the local HTTP server and CLI. `collector/core.py` manages collection import, normalization/deduplication, heuristics, review state, attribution exports and permitted local conversion. `collector/library.py` derives listening availability and honest presentation metadata. `collector/auth.py` owns accounts, sessions, account-private playlists/favorites and manually authorized creator bindings. `collector/location.py` parses explicit public state/location evidence; it does not prove current residence. `collector/model_classifier.py` optionally analyzes selected local evidence with external models.

The listening UI uses vanilla JavaScript/CSS. It is not a hosted SPA service. The Manifest V3 extension captures browser-visible public metadata and exports JSON for authenticated admin import; it does not receive Instagram passwords or bypass page restrictions.

## Storage

- `data/collection.db`: collection/review/source records, creator metadata and legacy shared playlists.
- `data/accounts.db`: password hashes, session hashes, private playlists/favorites, settings, invitation hashes, creator bindings, pending creator requests and minimal audit records.
- `data/media/`: local media/artwork. Playback is gated; source files are not automatically served.
- Other local JSON presentation/conversion reports can supply captured title/art/credit evidence. These are absent from the source-only repository.

Runtime storage is created locally and ignored by Git. The public tree includes no records or account database. A new installation therefore has an empty catalog and an unset official contact.

## Access model

Anonymous users: login/signup/setup UI and minimal auth state. Collection, media, review/export/admin and personal APIs require authentication.

Listeners: sanitized available accepted tracks and creator links; own playlists/favorites. Admin review data, private source URLs and other accounts' personal data are not available.

Creators: listener access plus specifically bound creator pages. May submit scoped correction/removal requests for human review; no direct source/collection mutation.

Admins: protected import/review/export tools, official contact configuration and verified invitation management. No public admin signup. Existing shared playlists remain preserved and accessible through a read-only admin endpoint, not silently assigned to a new account.

## Invitation lifecycle

1. Owner configures the real official Instagram handle.
2. Creator messages that account from the account being claimed.
3. Admin personally verifies identity/source/control, the intended existing local account, and a minimal evidence reference.
4. Admin issues a hashed, random, 24-hour, single-use token bound to the account and internal creator-page ID.
5. Admin manually delivers it in the verified DM. No email or automated messages are sent.
6. Recipient signs into the bound account and pastes the token; no token URLs are used.
7. Atomic redemption grants the scoped binding. Requests remain pending manual review.
8. Revocation removes granted access and invalidates that account's sessions. Conflicts require manual handling.

## Playback and layouts

The queue freezes when playback begins. Views and searches change the presentation, not the active audio source. MP3 byte ranges are handled by the authenticated server, and real `ended` events drive next-track playback.

Home defaults to Cards on initial load and each entry. List is still usable while on Home, but that temporary Home choice does not overwrite other sections' saved browser preference. Browse, favorites and playlists restore the saved preference. Creator directories remain grids, and access/admin forms do not show the performance toggle.

Duration values are derived from a loaded audio element; no all-track duration index ships with this release. Captured images are not official covers or verified profile photos, and caption-derived titles are not audio-verified.

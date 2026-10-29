# Changelog

Notable changes to Last.fm Radio for foobar2000 are recorded here, with the latest version first.

## 2.4.2 - 2026-10-29

### Added

- Info tab with Artist, Album and Track views.
- Playback following, focused-track lookup from Foobar, and pinned information from a local result's right-click menu.
- Artist biographies, album descriptions and track lists, track descriptions, tags and listening statistics where returned by Last.fm.
- Twelve preferred information languages, saved independently of the interface language.
- Bounded 24-hour response cache, refresh control, source links and text attribution.

### Fixed

- Artist and album descriptions missing in the preferred language now fall back to English, with an explicit notice.
- Empty or attribution-only text no longer prevents checking for an available summary or English description.

### Notes

- Local development versions 2.4.0 and 2.4.1 were not published separately; their changes are included here.
- The language fallback was checked against live Last.fm responses for John Wolfington. UI flows, language persistence and fallback rendering were tested with simulated responses; broader live Foobar testing remains ongoing.
- This update is available in the repository's `index.html`. Previously published release ZIPs are unchanged.

## [2.3.1] - 2026-10-29

### Added

- Right-click menu for locally matched tracks in Discovery, Tags and Users:
  - Open the track's Last.fm page.
  - Add the track to the current playlist.
  - Append the track to the playback queue.
  - Play the track after the current track by placing it at the front of the queue, keeping existing queued tracks in their original order.
- French and English labels for the new menu and action feedback.

### Notes

- The new actions do not edit audio metadata.
- Playlist additions are checked against the resulting playlist.
- Queue actions have been tested with local mocks; live foobar2000 confirmation is still pending.
- This update is available in the repository's `index.html`. Previously published release ZIPs are unchanged.

## [2.3.0] - Initial public beta

### Added

- Local-library playlist generation using Last.fm similar tracks, similar artists and artist top tracks.
- Multi-tag searches by artist or track, with tag-combination options and adjustable Surprise.
- Searches for locally available loved and most-played tracks from Last.fm users.
- Informational lists of missing artists and tracks.
- Album-level GENRE and COUNTRY suggestions from Last.fm, with preview, multi-value tag writing and readback verification.
- French and English interface with locally saved settings.
- Installation documentation and MIT license.

### Notes

- Earlier development versions are not reconstructed in this changelog.

[2.3.1]: https://github.com/thomasandro/foobar-lastfm-radio/commit/22229f79ff5d4560a7bea6a3911c5f27ad06fd5b
[2.3.0]: https://github.com/thomasandro/foobar-lastfm-radio/tree/22229f79ff5d4560a7bea6a3911c5f27ad06fd5b%5E

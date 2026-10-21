# Last.fm Radio for foobar2000

Version **2.3.0** · Windows · French / English interface

A local HTML panel for foobar2000's **foo_webview2** component. Discover music from your own library using Last.fm recommendations, tags and user charts, create playlists, and preview genre and country metadata before writing it to your files.

This is not a streaming player: it does not download audio or add unavailable Last.fm tracks to your playlists. It is not an official Last.fm application.

## Download

Download the ready-to-install ZIP from [Releases](https://github.com/thomasandro/foobar-lastfm-radio/releases). The first public release is a **beta**: feedback from clean installations is welcome.

## Screenshots

Screenshots show the actual HTML interface rendered in an isolated browser preview. The metadata example uses fictional sample data; it is not a screenshot of a completed tag write. No API keys or personal library data are shown.

### Discovery

![Discovery](https://raw.githubusercontent.com/thomasandro/foobar-lastfm-radio/main/docs/screenshots/discovery.png)

### Tag radio

![Tag radio](https://raw.githubusercontent.com/thomasandro/foobar-lastfm-radio/main/docs/screenshots/tag-radio.png)

### Genre and country preview — sample data

![Metadata preview with sample data](https://raw.githubusercontent.com/thomasandro/foobar-lastfm-radio/main/docs/screenshots/metadata-preview.png)

## Requirements

- foobar2000 for Windows, with a populated Media Library for discovery and matching.
- The [foo_webview2 component](https://www.foobar2000.org/components/view/foo_webview2). The script was developed for **0.3.27.0**, using its injected `fb2k` bridge and native host API. Other versions need testing; similarly named WebView components are not interchangeable.
- Microsoft Edge WebView2 Runtime, as required by the component.
- Internet access to Last.fm and a personal Last.fm API key.
- A Last.fm username for the Users features.

The reference setup uses foobar2000 v2, 64-bit, with **Default UI**. This is a beta intended for wider testing; compatibility with every foobar2000 configuration has not been verified. Spider Monkey Panel and a separate relay script are **not required** for this panel.

## Installation

1. Install `foo_webview2` through foobar2000's **Preferences → Components**, then restart foobar2000 if prompted.
2. Create a folder for the panel and place `index.html` in it. A recommended location is:

   ```text
   <foobar2000 profile>\webview\lastfm-radio\index.html
   ```

   For a standard installation, the profile is usually `%APPDATA%\foobar2000-v2`. A portable installation uses its own profile location. The script has no dependency on a particular Windows username or music drive.
3. In Default UI, enable **View → Layout → Enable layout editing mode**.
4. Add a **WebView2** UI element where you want the panel. Use a splitter or tab container first if necessary to preserve your existing layout.
5. In the WebView2 panel configuration, select the local `index.html` as its source. The exact configuration labels depend on the component version.
6. Disable layout editing mode.
7. Open the panel's **Settings / Paramètres** tab and configure your language and Last.fm account details.

The distributed HTML file contains the panel's styles and application code; no build step or external JavaScript file is required.

### Permissions

- **Write files** is required when using the buttons that write **GENRE** or **COUNTRY** metadata. Enable it in the WebView2 component/panel permissions if requested. The audio files themselves must also be writable.
- Discovery and playlist creation do not require metadata writing.
- This script does not launch PowerShell, an encoder or other external programs. It does not require shell-execution permissions.
- Allow the panel's HTTPS requests to `ws.audioscrobbler.com` through your network configuration. Last.fm pages are linked for reference.

Loading the local HTML file is separate from granting the script filesystem read/write API access. Do not enable unrelated permissions merely to troubleshoot an API-key or network error.

## Last.fm configuration

1. Sign in to Last.fm and [create an API account/key](https://www.last.fm/api/account/create).
2. Open **Settings** in the panel.
3. Enter your Last.fm username and your **API key**, then click **Save**. The current Save button requires both fields.
4. Do not enter your Last.fm password. A **Shared Secret is not required** by the current features.

The API key, username, language preference and other saved preferences are stored locally in WebView2's browser storage, not written into `index.html`. The panel does not read `apikey.txt` or `sharedsecret.txt`.

The API key is accessible to the JavaScript running in this panel. Use a trusted copy of the HTML. Clearing WebView2 browser data, changing its storage profile, or moving the HTML to a different location may require configuring the panel again.

### Language

Choose **Français** or **English** under **Settings → Langue / Language**. The choice is remembered locally. Switching language reloads the panel: saved settings remain, but the current preview and search results are cleared. Artist names, album titles and Last.fm tag values are not translated.

## Features and first use

### Discovery

1. Play a track in foobar2000.
2. Choose Automatic, Similar tracks, Similar artists, or Artist top tracks.
3. Set the requested number of tracks and the Surprise level, then search.
4. Review the selection and click **Create playlist**.

Higher Surprise settings broaden sampling; they do not guarantee that the requested number of tracks can be found locally. The panel creates a new, uniquely named playlist and verifies the added paths and their order. Creating a playlist makes it active.

Missing-artist and missing-track lists are informational. Matching depends on your local tags and may be affected by spelling variants, different editions and artist-name ambiguities.

### Tags

Search using up to six comma-separated tags, including multiword tags such as `modern classical`.

- **By artist** uses artist tags to select artists and their local tracks.
- **By track** uses track tags without substituting artist tags.
- **Any tag / All tags** controls how the requested tags are combined.
- Compound searches can also find candidates such as `jazz punk` when searching for `jazz, punk`.

Searches examine a limited candidate pool. A short result list does not prove that no other matching music exists on Last.fm or in your library. The maximum-per-artist setting can also reduce the final track count.

### Users

Refresh followed users, select a profile, and search for locally available loved or most-played tracks. You can also explore the following network to discover profiles with shared artists.

Compatibility scores are calculated by this panel and are not Last.fm website compatibility scores. Broader network searches can take longer. Unknown listening activity is distinguished from confirmed inactivity.

## Editing GENRE and COUNTRY

### Capture the albums

1. In foobar2000's active playlist, select one or more tracks from each album to process.
2. Open **Genres** and click **Analyse selection**.
3. Review every album's scope and track count before writing.

**For albums found in the Media Library, the scope is the entire matching album, including tracks you did not select.** Albums are grouped by album artist and album title. Multiple editions with identical grouping metadata may be included together; check the displayed track count.

Outside the library, select one complete album at a time. In that case, only the captured selected tracks are included. The panel cannot expand an album outside the library from a partial selection.

### Choose genres

Album tags are fetched first. The optional fallback uses artist tags when no suitable album tags are found. You can explicitly choose **Album tags** or **Artist tags**, enter a different artist name, or search individual proposed collaboration artists.

The preview preselects up to three, four or five suitable tags. Years, common personal/descriptive labels and recognised country tags are excluded from automatic genre selection, but remain visible. Last.fm tags are user-generated: always review the suggestions.

For collaborations, individual **Genres: artist name** buttons and **Find genres for all proposed artists** are available. The grouped search merges tags without duplicates and shows their artist provenance. Switching the genre source replaces the genre suggestions in the preview; it does not write tags.

### Choose countries

Countries are searched independently in **artist tags**, even when the genre preview uses album tags. Exact aliases are normalised to English country names, for example:

- `Germany`, `German`, `Deutschland`, `deutsch` → `Germany`
- `Japan`, `Japanese`, `日本` → `Japan`
- `Ghana`, `Ghanaian` → `Ghana`

The alias table is deliberately limited rather than an exhaustive list of every country in every language. Compound style tags such as `german techno` are not converted into countries.

Country suggestions are **unchecked by default** and show their source artist/tag. Adjectives may describe a language rather than an artist's origin. COUNTRY is intended here as artist origin, not the album's manufacturing or release territory.

For collaborations, query individual artists or use **Find countries for all proposed artists**. You can retain several countries for an international collaboration.

### Collaboration-name suggestions

The script proposes splitting on `&`, `feat`, `feat.`, `featuring`, `ft`, `ft.`, `with`, `vs`, `vs.`, `;`, `/`, `+`, and a hyphen surrounded by spaces (` - `). Matching is case-insensitive. `/`, `+` and `;` also work without surrounding spaces.

These are suggestions, not verified identities. A band name may contain one of these characters. Keep the full artist name or edit the search field when necessary. Names containing a plain internal hyphen, such as `Jean-Michel`, are not split on that hyphen.

### Apply and verify

The buttons are below the results, together with progress, confirmation and error messages:

- **Add GENRE / Add COUNTRY** combines the checked suggestions with the existing values across the album's included tracks, removes case-insensitive duplicates, and writes the same combined values to all included tracks.
- **Replace GENRE / Replace COUNTRY** writes only the checked values for that field to all included tracks.
- Albums with no checked values for that field are skipped; an empty selection does not clear an existing field.

GENRE and COUNTRY are written separately, using real multi-value fields. The panel rereads each field to verify the requested values. A request accepted by foobar2000 is not reported as fully verified until this check succeeds.

If verification times out, check the files' properties before retrying: some writes may already have completed. Multiple-album writes are sequential, not an all-or-nothing transaction. The status reports albums completed before an error. Metadata changes detected since the preview require a fresh analysis.

No audio encoding, file moving or deletion is performed by this panel.

## Cache and updates

Genre/country API responses are cached locally for up to seven days, with a bounded cache. Other search features also use caches. Recent Last.fm tag changes may therefore not appear immediately.

To update, keep a backup of `index.html`, replace it with the new version in the **same location**, and reload the WebView2 panel. Saved browser-storage preferences normally remain. Check the version number at the top of the panel. Reloading clears current results and metadata previews; finish pending writes first.

## Troubleshooting

| Symptom | What to check |
| --- | --- |
| Blank panel or missing `fb2k` / host API | Open the HTML inside the correct foo_webview2 panel, not a normal browser. Check the component version and WebView2 Runtime. |
| Missing or rejected API key | Enter your personal API key in Settings and save. A Shared Secret is not the API key. |
| Last.fm timeout or HTTP error | Check your connection, firewall and Last.fm availability. Retry later for service/rate-limit errors. |
| Few or no tracks found | Check the Media Library, local artist/title tags, candidate limits, tag combination and per-artist maximum. |
| Album outside the library | Select that complete album alone, or add it to the Media Library. |
| Unexpected album size | Check for multiple editions sharing the same album artist and album title before writing. |
| Write permission error | Enable Write files and check file permissions. |
| File in use / write not confirmed | Check the file in foobar2000 Properties. Stop playback if it is locking the file, then analyse again as necessary. |
| No country suggestion | Query the correct individual artist. Their Last.fm tags may contain no recognised country. |
| Saved settings disappeared | Check whether the HTML location, WebView2 profile or browser data changed. |

When reporting a problem, include the script version, foobar2000 version/architecture, foo_webview2 version, the feature used, and the exact message. A screenshot can help. **Never include your API key, password or Shared Secret.**

## Sharing this script

Distribute the intended public files (`index.html`, `README.md` and `LICENSE`), not an entire personal installation folder. Exclude credential files, old HTML copies, browser data and logs with private information.

## License and credits

Copyright (c) 2026 **gloq**.

This script and its documentation are distributed under the **MIT License**. See [LICENSE](LICENSE) for the full terms.

- Project maintained by **gloq**.
- Developed by gloq with assistance from OpenAI Codex.
- Built for foobar2000 using the [foo_webview2 component](https://www.foobar2000.org/components/view/foo_webview2). The component and foobar2000 are separate projects and are not included in this distribution.
- Music metadata and recommendations are provided by [Last.fm](https://www.last.fm/). API use and Last.fm data remain subject to [Last.fm's API terms](https://www.last.fm/api/tos); the script's MIT License does not relicense that data.

This project is independent and is not endorsed by Last.fm or the foobar2000/component developers.



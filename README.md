# Hypnotix
![build](https://github.com/linuxmint/hypnotix/actions/workflows/build.yml/badge.svg)

Hypnotix is an IPTV streaming application with support for live TV, movies and series.

![shadow](https://user-images.githubusercontent.com/1138515/99553152-b8bac780-29b5-11eb-9d75-8756ed7581b6.png)

It can support multiple IPTV providers of the following types:

- M3U URL
- Xtream API
- Local M3U playlist

# License

- Code: GPLv3
- Flags: https://github.com/linuxmint/flags
- Icons on the landing page: CC BY-ND 2.0

# Requirements

- libxapp 2.6+
- libmpv
- python3-imdbpy (for Older Mint and Debian releases get it from https://packages.ubuntu.com/focal/all/python3-imdbpy/download)

# TV Channels and media content

Hypnotix does not provide content or TV channels, it is a player application which streams from IPTV providers.

By default, Hypnotix is configured with one IPTV provider called Free-TV: https://github.com/Free-TV/IPTV.

This provider was chosen because it satisfied the following criterias:

- It only includes free, legal, publicly available content
- It groups TV channels by countries
- It doesn't include adult content

Issues relating to TV channels and media content should be addressed directly to the relevant provider.

Note: Feel free to remove Free-TV from Hypnotix if you don't use it, or add any other provider you may have access to or local M3U playlists.

# Wayland compatibility

If you're using Wayland go the Hypnotix preferences and add the following to the list of MPV options:

`vo=x11`

Run Hypnotix with:

`GDK_BACKEND=x11 hypnotix`


# 🎥 Hypnotix Remix — The Next-Gen IPTV Experience for Linux

A community-powered remix of [Linux Mint Hypnotix](https://github.com/linuxmint/hypnotix) — rebuilt for maximum customization, performance, and fun.  
Stream IPTV, movies, and series your way — with zero ads, zero tracking, and infinite remix potential.

---

## Why Hypnotix Remix is Different
- **Stream Everything** — IPTV, movies, series, and live channels from any M3U playlist.
- **Auto-Organized Channels** — Groups by country, genre, or your own custom filters.
- **Beautiful EPG Integration** — See what’s on now and what’s next.
- **Custom Skins & Themes** — Dark mode, minimal mode, and color packs.
- **Plugin System** — Add your own content sources, filters, or viewing modes.
- **Global Content Search** — Search across all playlists instantly.
- **One-Click Recording** — Save your favorite shows offline.
- **Multi-OS Ready** — Optimized for Linux Mint but works across Linux.

---

##  What's Coming Next
- AI-based content recommendations
- Multi-screen mode for sports events
- Real-time chat & watch parties
- Voice-controlled channel switching
- Automatic subtitle downloads
- Local network streaming to smart TVs
- Touchscreen optimization for 2-in-1 devices

---

## Why Choose Linux Mint with Hypnotix Remix?
Linux Mint is a clean, stable, and user-friendly desktop — Hypnotix Remix makes it your ultimate entertainment hub.

- **No spyware, tracking, or bloated ads**
- **Better performance than browser-based IPTV**
- **Customizable to your workflow**
- **Community-driven development**

---

## Running from Source
```bash
python3 usr/bin/hypnotix

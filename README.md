<p align="center">
  <img src="logo/ldm.svg" alt="LDM logo" width="128" height="128">
</p>

<h1 align="center">LDM - Linux Download Manager</h1>

<p align="center">
  A fast multi-connection download manager for Linux with a classic, easy-to-use interface,
  video downloading and browser integration for Firefox and Chromium-based browsers.<br>
  <sub>Copyright (C) 2026 <b>SL_Linux</b> - licensed under the GNU GPL v3 or later</sub>
</p>

![LDM main window](docs/screenshot.png)

## Features

* **Faster downloads** - each file is split into segments fetched over several connections
  (up to 32). When a connection finishes, it takes over part of the slowest remaining segment so
  every connection stays busy.
* **Pause, resume and recover** - progress survives closing the app. Servers that refuse many
  connections are handled by backing off and retrying automatically.
* **Queues, speed limit and categories** - downloads are sorted into `Compressed`, `Documents`,
  `Music`, `Programs`, `Video` and `General` folders.
* **Video downloads** - YouTube, Vimeo, Facebook and many other sites through
  [yt-dlp](https://github.com/yt-dlp/yt-dlp), with a quality picker. LDM can install yt-dlp and
  the JavaScript runtime it needs for you (checksum-verified).
* **Multi-server streaming sites** - the browser extension detects the HLS / DASH / MP4 stream
  a player is actually loading and hands it to LDM with the same Referer, Origin and User-Agent
  the browser used.
* **Browser integration** - a download button on the video, a *Download with LDM* right-click
  entry, and capture of browser downloads. Works with Firefox, Chrome, Chromium, Brave, Edge and
  Vivaldi.
* **Tray icon and background mode** - LDM keeps listening to the browser after you close the window.
* **Normal-application behavior** - the prompt opens centred and in front of the browser, and
  *Open* / *Open Folder* on the completion window close it and show the file.

### Supported file types

| Category | Types |
| --- | --- |
| Compressed / archives | ZIP, RAR, 7Z, TAR, GZ, GZIP, ISO, IMG, MSI, EXE |
| Documents | DOC, DOCX, PDF, PPT, PPTX, XLS, XLSX, TXT |
| Music / audio | MP3, WAV, WMA, AAC, M4A, OGG, AIF |
| Video / movies | MP4, MKV, AVI, MOV, FLV, WMV, WEBM, 3GP, M4V, TS |

Any other file also downloads; unknown types go to `General`.

## Supported Linux distributions

LDM is pure Python 3 (3.8 or newer) + GTK 3, so it runs on any distribution that has those.

| Distribution family | How to install | Status |
| --- | --- | --- |
| Debian, Ubuntu, Linux Mint, Pop!_OS, Zorin, elementary | `.deb` package | Built and installed on Ubuntu 24.04 |
| Fedora, RHEL / Rocky / Alma, openSUSE | `.rpm` package | Built here; not yet installed on these systems |
| Arch, Manjaro, EndeavourOS | `PKGBUILD` | Provided; not yet tested with `makepkg` |
| Any other (Alpine, Void, Gentoo, NixOS, ...) | `install.sh` | Dependency names provided; not yet tested |

"Built here" means the package builds and has the right contents. Reports from other distributions
are welcome. Desktop notes: the tray icon needs a tray (Cinnamon, MATE, XFCE, KDE) or the
AppIndicator extension on GNOME; without one LDM still works and shows a notification when it goes to
the background. On Wayland the window-raising features are limited by the compositor.

See **[INSTALL.md](INSTALL.md)** for step-by-step instructions.

## Quick start

```sh
git clone https://github.com/YOUR-USERNAME/ldm.git
cd ldm
./install.sh --deps        # installs the needed system packages, then LDM into ~/.local
ldm
```

Then load the browser extension (see [INSTALL.md](INSTALL.md#browser-extension)).

## How the browser integration works

The extension talks to the running LDM over a private local link on `127.0.0.1`
(ports 38477-38481) that only accepts the LDM extension. If LDM is not running it asks the
desktop to start it through an `ldm://` link; native messaging is kept as an extra path. Nothing is
sent to any server except the sites you download from.

## Project layout

```
bin/ldm                     launcher
src/ldm/                    the application (core engine, GTK interface, local link)
libexec/ldm-native-host     browser native-messaging helper
extension/{chrome,firefox}  the browser extension
data/                       desktop entry and icons
logo/                       logo artwork and guidelines sheet
packaging/                  Debian, RPM and Arch packaging files
install.sh                  installer for any distribution (also used by the packages)
build-deb.sh build-rpm.sh   package builders
```

## Build packages

```sh
./build-deb.sh      # -> ldm_<version>_all.deb          (needs dpkg-deb)
./build-rpm.sh      # -> ldm-<version>-1.noarch.rpm     (needs rpmbuild)
```

## Branding

The icon and its rules (clear space = one quarter of the icon width; minimum size 32 px full /
16 px compact) are in [docs/LOGO-GUIDELINES.md](docs/LOGO-GUIDELINES.md).

## Responsible use and limits

* Use LDM only for content you have the right to download. LDM does **not** bypass copy
  protection: streams protected by DRM are reported and not downloaded.
* LDM relies on third-party tools you install through it (yt-dlp, Deno) and on ffmpeg; their
  licenses and terms apply.
* Known areas to harden are tracked in the project issues (local-link authentication, redirect
  handling, log privacy).

## License and credits

Copyright (C) 2026 **SL_Linux**. LDM is free software under the
[GNU General Public License v3.0 or later](LICENSE). Created by SL_Linux.

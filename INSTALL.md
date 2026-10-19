# Installing LDM

Copyright (C) 2026 SL_Linux

LDM needs **Python 3.8+**, **GTK 3 with its Python bindings (PyGObject)** and **pycairo**.
`ffmpeg` is needed to merge video and audio streams. Pick the section for your distribution.

* [Debian, Ubuntu, Linux Mint and relatives](#debian-ubuntu-linux-mint-pop_os-zorin)
* [Fedora, RHEL, Rocky, Alma, openSUSE](#fedora-rhel-rocky-alma-opensuse)
* [Arch, Manjaro, EndeavourOS](#arch-manjaro-endeavouros)
* [Any other distribution](#any-other-distribution-installsh)
* [Browser extension](#browser-extension) - needed for the in-browser download buttons
* [Video downloads](#video-downloads) - first-run setup
* [Update, uninstall, troubleshooting](#update-and-uninstall)

## Debian, Ubuntu, Linux Mint, Pop!_OS, Zorin

```sh
sudo apt install ./ldm_1.7.0_all.deb
```

`apt` pulls in the dependencies. Optional extras: `sudo apt install ffmpeg wmctrl`.
To build the package yourself from this repository: `./build-deb.sh`.

## Fedora, RHEL, Rocky, Alma, openSUSE

```sh
sudo dnf install ./ldm-1.7.0-1.noarch.rpm        # Fedora, RHEL family
sudo zypper install ./ldm-1.7.0-1.noarch.rpm     # openSUSE
```

For video merging install ffmpeg: Fedora has `ffmpeg-free`, or use `ffmpeg` from RPM Fusion;
openSUSE: `sudo zypper install ffmpeg` (Packman repository).
To build the package yourself: `./build-rpm.sh` (needs `rpm-build`).

## Arch, Manjaro, EndeavourOS

```sh
tar czf ldm-1.7.0.tar.gz --transform 's,^\./,ldm-1.7.0/,' .     # inside the repository
cp packaging/arch/PKGBUILD . && makepkg -si
```

Optional: `sudo pacman -S ffmpeg wmctrl libayatana-appindicator`.

## Any other distribution (`install.sh`)

```sh
./install.sh --deps            # install system packages (asks for sudo), then install LDM for you
```

Or in two steps:

```sh
./install.sh --deps-only       # only the system packages (add --dry-run to just print the command)
./install.sh                   # per-user install into ~/.local - no root needed
sudo ./install.sh              # system-wide install into /usr/local
```

`--deps` knows apt, dnf, zypper, pacman, apk (Alpine), xbps (Void) and emerge (Gentoo). Equivalent
packages if you prefer to install them by hand:

| Distribution | Packages |
| --- | --- |
| Debian / Ubuntu | `python3 python3-gi python3-gi-cairo gir1.2-gtk-3.0 gir1.2-gdkpixbuf-2.0 xdg-utils ffmpeg` |
| Fedora / RHEL | `python3 python3-gobject python3-cairo gtk3 xdg-utils` (+ ffmpeg) |
| openSUSE | `python3 python3-gobject python3-gobject-Gdk python3-cairo typelib-1_0-Gtk-3_0 xdg-utils ffmpeg` |
| Arch | `python python-gobject python-cairo gtk3 xdg-utils ffmpeg` |
| Alpine | `python3 py3-gobject3 py3-cairo gtk+3.0 xdg-utils ffmpeg` |
| Void | `python3 python3-gobject python3-cairo gtk+3 xdg-utils ffmpeg` |
| Gentoo | `dev-python/pygobject dev-python/pycairo x11-libs/gtk+:3 x11-misc/xdg-utils media-video/ffmpeg` |
| NixOS | `python3`, `python3Packages.pygobject3`, `gtk3`, `gobject-introspection`, `xdg-utils`, `ffmpeg` |

Options: `--prefix DIR` to choose the location, `--user` / `--system`, `--uninstall`.
After a per-user install, make sure `~/.local/bin` is on your `PATH` to type `ldm` in a
terminal (the application-menu entry works without it).

## Browser extension

The extension adds a download button on videos, a *Download with LDM* right-click entry and
capture of browser downloads. Keep LDM running (it stays in the background when you close its
window).

The extension files are installed with LDM:

| Install type | Extension folder |
| --- | --- |
| `.deb` / `.rpm` / Arch | `/usr/share/ldm/extension/` |
| `sudo ./install.sh` | `/usr/local/share/ldm/extension/` |
| `./install.sh` (per-user) | `~/.local/share/ldm/extension/` |

**Chrome, Chromium, Brave, Edge, Vivaldi**

1. Open `chrome://extensions` (Brave: `brave://extensions`) and turn on **Developer mode**.
2. Click **Load unpacked** and choose the `chrome` folder from the table above.

**Firefox**

1. Open `about:debugging#/runtime/this-firefox`.
2. Click **Load Temporary Add-on...** and choose `firefox/manifest.json`.

Firefox removes temporary add-ons when it closes. For a permanent install the add-on has to be
signed by Mozilla (a free "unlisted" submission on addons.mozilla.org); then install the signed
`.xpi` file.

After updating LDM, reload the extension (the popup warns when its version no longer matches).
The popup's **Test connection to LDM** button checks that everything is linked.

*Optional:* LDM > Tasks > Browser integration > **Register for my browsers** lets the browser
start LDM itself. It is not required; the local link works in every browser, including ones where
native messaging is unavailable (snap and Flatpak builds).

## Video downloads

The first time you download a video, LDM offers to install **yt-dlp** and **Deno** (a JavaScript
runtime YouTube now requires) from their official GitHub releases into
`~/.local/share/ldm/bin`, verifying checksums. Use *Tasks > Install / update video downloader*
to update them later. Install **ffmpeg** from your package manager so video and audio can be merged.

If a site asks you to sign in, choose your browser under *Options > Use this browser's login
cookies for video sites*.

## Update and uninstall

* Packages: install the newer `.deb` / `.rpm` the same way (or `makepkg -si` again); remove with
  `sudo apt remove ldm`, `sudo dnf remove ldm`, `sudo zypper remove ldm` or `sudo pacman -R ldm`.
* `install.sh`: run it again to update; remove with `./install.sh --uninstall`
  (use the same `--user`/`--system`/`--prefix` options as when installing).
* Your settings and download list live in `~/.config/ldm` and `~/.local/share/ldm`; they are never
  deleted automatically.

## Troubleshooting

| Problem | What to do |
| --- | --- |
| Popup says *Not connected* | Start LDM (it must be running), then press **Test connection** again. |
| Nothing visible after closing the window | LDM keeps running in the background. Open it from the applications menu or the tray icon; on GNOME install an AppIndicator extension to get a tray icon. |
| Prompt appears behind the browser | Install `wmctrl`; on Wayland compositors raising windows from another app can be limited. |
| `No such native application` | Harmless: the local link is used instead. Only the optional *Register* button needs this. |
| Video download fails with a JavaScript error | Run *Tasks > Install / update video downloader* to get Deno. |
| `ModuleNotFoundError: gi` | Install the PyGObject package for your distribution (see the table above). |

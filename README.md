Rukbi Keyboard Layouts
======================

https://ilyabirman.ru/typography-layout/

Rukbi keyboard layouts enable a user to type various useful characters like quotation marks, en and em dashes, arrows,
as well as to use one combined layout for a pair of languages (Russian and Ukrainian, English and German).

To type additional characters, `AltGr` (right `Alt`) is used: thus, pressing `AltGr` + `<` will result in a quotation
mark: `«`.

## Installation and configuration

Rukbi is packaged for **libxkbcommon ≥ 1.13 / xkeyboard-config ≥ 2.45** using the
native XKB *extensions directories* mechanism. The layout files are installed
into `/usr/share/xkeyboard-config.d/rukbi/`, which `xkbcommon` (used by
`kwin_wayland` to compile keymaps) and `libxkbregistry` (used by KDE System
Settings, GNOME, `xkbcli`) scan automatically.

No patching of xkeyboard-config files: updating `xkeyboard-config` never
reverts anything, and removing the package cleans up completely. The install
script only prints a reminder to restart the session (see below); it modifies
nothing.

### Build and install

    cd arch-build
    makepkg -si

or, to build the package file only:

    makepkg -f
    sudo pacman -U rukbi-5.0-1-any.pkg.tar.zst

The install prints a reminder to restart the session before adding the layouts
(required, see *Configuration* below).

The build downloads the code of the `v5.0` tag from the upstream repository;
the layout registry (`evdev-rukbi.xml`) is shipped next to the PKGBUILD.

Requirements (all are satisfied on current Arch):

* `libxkbcommon` ≥ 1.13 (extensions directories support)
* `xkeyboard-config` ≥ 2.45
* `base-devel` and `git` for building from source

### Configuration (KDE Plasma / Wayland)

1. **Install the package, then start the session** (log out and back in if it
   is already running). `kwin_wayland` scans the XKB extension directories
   exactly once, when it starts; layouts installed into an already-running
   session are invisible to it, the new keymap fails to compile, and KDE falls
   back to a single default layout without any layout-switch options. There is
   no way to apply a newly installed layout package to a running
   `kwin_wayland` — restarting the session is required (do not use
   `systemctl --user restart plasma-kwin_wayland.service`, it terminates the
   session).
   The System Settings layout dialog runs in a separate process and therefore
   shows the new layouts immediately, even though the running session cannot
   use them yet.
2. Open **System Settings → Keyboard → Layouts → Add layout**.
3. Choose one of the Rukbi entries (they appear in the list automatically,
   together with their variants).
4. Keep the total number of layouts at **four or fewer**: an XKB keymap
   supports at most 4 groups, so the 5th and 6th layout are silently dropped
   by `xkbcommon`.

Layouts and variants:

| Layout      | Description                |
|-------------|----------------------------|
| eng         | English                    |
| eng(deu)    | English + Deutsch          |
| deu         | Deutsch                    |
| rus         | Russian                    |
| rus(ukr)    | Russian + Ukrainian        |
| ukr         | Ukrainian                  |
| ukr(rus)    | Ukrainian + Russian        |

`AltGr` (right `Alt`) gives access to typographic characters, e.g.
`AltGr` + `<` → `«`.

> **Note**: Layout names are short ISO 639 codes (`eng`, `deu`, `rus`, `ukr`).
> This is also what KDE shows as the per-layout short name in System Settings
> and the tray indicator, so no manual renaming is needed. The two-letter
> codes `en` and `uk` are free, but `de`/`ru` would collide with the stock
> layouts of the same names bundled with `xkeyboard-config`, so the ISO 639-2
> codes `deu`/`rus` are used instead.

> **Note**: This package is designed for **Wayland only** (libxkbcommon ≥ 1.13).
> Xorg does not support the extensions directories mechanism; for X11-only
> sessions this package does not provide an installer.

### Removing Rukbi

    sudo pacman -Rs rukbi

That's it — no leftover patches to clean up.
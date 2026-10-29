# kdeconnect-minimal

[KDE Connect](https://invent.kde.org/network/kdeconnect-kde) without KIO, Kirigami,
KPeople, Qt Quick or Qt Multimedia: the daemon (`kdeconnectd`), `kdeconnect-cli`
and the plugins that need no GUI. Made for desktops that drive KDE Connect over
D-Bus from their own shell (Quickshell, waybar, ...) and do not want the KDE
frontends and their dependency tree.

This is a fork. The `minimal` branch is upstream's release tag plus a handful of
commits on top; `git log v26.08.2..minimal` is the whole difference.

## Removed

- Frontends: the Kirigami app, SMS app, indicator, Plasma applet, KIO slave,
  Dolphin / Nautilus integrations, URL handler.
- Plugins: digitizer, presenter, SMS, virtual monitor,
  share-input-devices, send-notifications, find-this-device.
- Bluetooth transport (`-DBLUETOOTH_ENABLED=OFF`) and X11 (`-DWITH_X11=OFF`).

## Changed

- `core/openconfig.cpp`: starts `kdeconnect-app` with `QProcess`.
- `plugins/share`: opens received files and text with `QDesktopServices`.
- `plugins/sftp`: no Dolphin Places entry; "browse" opens the mount point.
- `daemon/`: no KIO job tracker, no Qt Quick style setup.

## Building

    cmake -B build -DBUILD_TESTING=OFF -DWITH_X11=OFF -DBLUETOOTH_ENABLED=OFF \
          -DINSTALL_UFW_APPLICATION_RULE=OFF
    cmake --build build --parallel 2

Needs `extra-cmake-modules`, `wayland-protocols`, Qt 6 base and the KF6
frameworks CMake asks for. `pulseaudio-qt` enables `systemvolume` and
`pausemusic`; pass `-DWITH_PULSEAUDIO=OFF` to leave them out.

## Moving to a new upstream release

    git remote add upstream https://invent.kde.org/network/kdeconnect-kde.git
    git fetch upstream --tags
    git rebase --onto vNEW vOLD minimal
    git tag vNEW-minimal.1

Arch: `kdeconnect-minimal` on the AUR builds from these tags.

Licensing is upstream's: see `LICENSES/`.

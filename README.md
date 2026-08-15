# Camural — releases

Camural puts your own UniFi® Protect cameras on your Windows desktop and fires a notification
with a camera snapshot when something happens. One-time purchase, no account, no cloud, no
telemetry.

**→ [camural.com](https://camural.com)** — what it does, screenshots, and where to buy it.

## What this repository is

Downloads only. Every published version lives under
[Releases](https://github.com/camural/releases/releases), and Camural checks this page once a
day to see whether a newer build exists. That is the only request Camural makes on its own to
anything other than your own console, and it is described in the
[privacy policy](https://camural.com/privacy.html).

There is no source code here. Camural is proprietary software licensed under the terms shown
during installation.

## Installing

Download the installer from the latest
[release](https://github.com/camural/releases/releases/latest) and run it. It installs for the
current user and needs no administrator rights — Camural never runs elevated, because Windows
silently discards notifications from elevated processes.

Updates are downloaded quietly in the background and installed the next time you exit Camural.
It will never restart itself to apply one: a camera viewer that vanishes mid-event is a viewer
that missed the event.

## Open-source components

Camural plays video with [libmpv](https://mpv.io/), used unmodified under the **LGPL v2.1** and
dynamically linked. The unmodified, replaceable `libmpv-2.dll` ships beside the executable, and
the full licence text is installed alongside it. The remaining third-party components and their
licences are listed in `THIRD-PARTY-NOTICES.md` inside the installation folder.

## Support

**support@camural.com**

When reporting a problem, attach the log from `%LOCALAPPDATA%\Camural\logs` — it records what
Camural did, and never your API key, your stream credentials or your camera images.

---

UniFi and UniFi Protect are trademarks of Ubiquiti Inc. Camural is an independent product and is
not affiliated with, endorsed by, or sponsored by Ubiquiti Inc.

# UniFi Protect cameras on your Windows desktop

Camural is a small Windows tray app for UniFi&reg; Protect. It gives every camera on your console
its own window on the desktop, and raises a Windows notification with the camera's snapshot already
inside it when something happens. Video travels from your own console to your PC and stops there.
No account, no cloud relay, no telemetry, no crash reporting, nothing to subscribe to.

$14.99 once, with a 14-day trial that needs no card. Everything about the product, including the
purchase page, is at [camural.com](https://camural.com).

![A Windows desktop with three Camural camera windows docked in a column against the right-hand
edge.](https://camural.com/img/hero-desktop.jpg)

## What this repository is

Installers and an issue tracker. Every published version is under
[Releases](https://github.com/Camural/releases/releases), and an installed copy of Camural asks this
page once a day whether a newer build exists. That daily check is the only request the app makes to
anything other than your own console, and it is described in the
[privacy policy](https://camural.com/privacy.html).

There is no source code here and none is coming. Camural is proprietary software, licensed under the
terms shown during installation. The screenshots on this page are served from camural.com rather
than from the repository, because nothing but release assets is stored here.

## Download

**[CamuralApp-win-Setup.exe](https://github.com/Camural/releases/releases/latest/download/CamuralApp-win-Setup.exe)**,
about 119 MB, which Windows will show as 113 MB. That link always resolves to the newest release, so
it never goes stale.

The installer puts Camural in `%LOCALAPPDATA%\CamuralApp` for your Windows user alone and never asks
for administrator. Do not run it elevated: Windows silently discards notifications raised by
elevated programs, which breaks the main reason to install it. Builds are signed by Seth Walker
through Azure Trusted Signing.

Updates download quietly in the background and are applied when you next exit, never mid-session.
Before applying one, Camural copies your console details, per-camera rules, confirmed certificates,
layouts and licence into a dated backup folder, and keeps the last ten.

## What it does

- Every camera opens in its own frameless window you drag where you want. Windows snap to screen
  edges and to each other, keep the camera's own aspect ratio instead of stretching it, so a 4:3
  doorbell never ends up in a 16:9 box, and belong to a monitor rather than a coordinate, so
  unplugging a screen parks those cameras instead of leaving them decoding off the edge of the
  desktop.
- Per camera, from a right-click menu: always on top, locked in place, four corner presets, and
  which stream quality it pulls. `Ctrl` + `Alt` + `C` raises every camera above your other windows,
  and pressing it again puts them back.
- Double-click a camera for a full-size watch window on its high-quality stream.
- A camera wall drawn on the Windows wallpaper layer, behind your icons and behind every window,
  costing no screen space. You place the tiles on a scaled model of your actual monitors. Camural
  does not replace your wallpaper. It draws tiles over it and leaves the rest showing.
- PTZ cameras point from a bar of saved positions, from the right-click menu, or from the number
  keys in the watch window. Patrols start and stop from the same menu. A game controller (Xbox,
  DualSense or DualShock 4) drives pan, tilt and zoom, but only while the watch window is the one
  you are using, so a pad plugged in for a game cannot pan a camera.
- Notifications carry the frame that triggered them, full width, above the camera name. Motion,
  person, vehicle, animal, package, licence plate, loitering, line crossing and doorbell rings, plus
  the sounds Protect recognises: smoke alarms, carbon monoxide alarms, glass breaking, sirens, a
  baby crying and barking.
- Which of those raise a notification is set per camera, so plain motion can be off on the
  road-facing camera while people stay on. Audio alarms have a switch of their own, because turning
  off motion should never turn off the alert about a fire.
- Alert history for the last 24 hours with the snapshots kept alongside, because Windows drops
  banners while you are in a game or presenting, and history is then the only record.
- Several Protect consoles at once, each camera routed to the console that owns it, so two sites
  that both have a Front Door never collide over alerts, rules or layout.
- Hardware decoding through Direct3D, the low-bandwidth substream by default in the grid, and pause
  rules that stop the decoders when the screen is covered, locked, on battery or in energy saver.

<img src="https://camural.com/img/alert-toast.png" width="400" alt="A Windows notification from Camural showing a person at a front door.">

Alerts and alert history are never licence-gated, in any state: not during the trial, not after it,
not if the licence file is corrupted. Two tests in the build fail if the alert pipeline or the toast
service so much as imports the licensing code.

## What it does not do

- It does not record. No timeline, no playback, no stored video. Your console keeps doing all of
  that, and Camural does not replace an NVR.
- It has no cloud side. No Camural account, no relay, no analytics, no licence server. Reaching your
  cameras from outside your network needs a VPN you set up yourself.
- No two-way audio, and it will not answer your doorbell. It tells you the doorbell rang and shows
  you who is there.
- Camera windows and the wall pause over Remote Desktop, on purpose. Alerts keep working.
- It conflicts with Wallpaper Engine and Lively. Two apps cannot own the wallpaper layer at once, so
  use one or the other. Placed camera windows and alerts are unaffected either way.
- It is not affiliated with Ubiquiti. Support comes from the author, not from them.

## Will it run on your setup

Windows 10 version 1809 or later, or Windows 11, 64-bit x64 only. Not Windows on ARM, and not
32-bit. UniFi Protect 5.3.38 or newer, which is the first firmware to expose the official
integration API and so a hard floor. Update well past it if you can, to Protect 7.1.83 or later, or
6.2.72 on the 6.x line, with UniFi OS 5.1.19. 4 GB of RAM, 8 GB recommended. Any DirectX 11 GPU that
decodes H.264 in hardware, and one that decodes H.265 if you want a wall of live cameras to be
comfortable. 1 GB of free disk. Your PC has to be able to reach the console, on your own network or
over your own VPN.

You also need an API key that you make yourself on the console, under Settings, then Control Plane,
then Integrations. That page is hidden from limited accounts, so you need a Super Admin or Owner
account, and the key is shown once. A key made at unifi.ui.com is a different, cloud-side key and
will not work. The key has to be able to write as well as read, because Protect will not serve a
stream for a camera with no RTSPS stream enabled, so Camural enables one the first time that camera
plays live video. Driving a PTZ camera live and saving a new preset need a local console username
and password as well, which is also what lets Camural find your PTZ cameras and name their saved
positions.
[Every write it makes is listed on the compatibility page](https://camural.com/compatibility.html#writes),
along with the full requirements table.

## Setting it up

Run the installer, press Find or type your console's address, confirm the certificate fingerprint
Camural shows you, then paste your API key. Nothing is sent to the console until you have confirmed
that fingerprint.

The full walkthrough, with every window pictured, is the manual:
**[camural.com/how-to-use.html](https://camural.com/how-to-use.html)**.

## What it costs

$14.99 in US dollars, once. One licence covers one person on every Windows PC they own, and it is
verified offline against a key built into the app, so there is no activation server and nothing that
can withdraw it later. Stripe is the merchant of record, so Stripe is what appears on your card
statement, and it handles tax, invoices and refunds within 30 days.

The trial is 14 days with nothing held back, and it needs no card and no sign-up. The clock starts
the first time Camural reaches one of your consoles, not when you download it, so a week spent
finding your API key does not cost you a week of trial.

What changes when the trial ends:

- Alerts and alert history keep working, and always will.
- Camera windows on the desktop go from as many as you like to one at a time.
- The camera wall on the wallpaper switches off.
- The PTZ controls stop moving cameras. Stopping a patrol already running still works.
- Your settings, layouts and per-camera rules are kept either way, and come back the moment you
  enter a key.

## Reporting a problem

Email **support@camural.com**, or open an issue here. Both reach the same person.

Include the version from the top of the tray menu, your Windows version and your Protect version,
and attach the log from `%LOCALAPPDATA%\Camural\logs`. The log records what Camural did, and never
your API key, your console password, your stream credentials or your camera images.

## Open-source components

Camural plays video with [libmpv](https://mpv.io/), used unmodified under the LGPL v2.1 and
dynamically linked. The unmodified, replaceable `libmpv-2.dll` ships beside the executable, and the
full licence text is installed alongside it. The remaining third-party components and their licences
are listed in `THIRD-PARTY-NOTICES.md` inside the installation folder.

---

UniFi&reg; is a registered trademark, and UniFi Protect&trade; a trademark, of Ubiquiti Inc. in the
United States and other countries. Camural is an independent product, written and supported by Seth
Walker trading as Walker Computers, and is not affiliated with, endorsed by or sponsored by Ubiquiti
Inc.

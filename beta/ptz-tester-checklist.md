<!-- Published for beta testers. The working copy lives in the source repository at
     docs/notes/ptz-tester-checklist.md; edit it there rather than here. -->

# PTZ: what to check on real hardware

For a beta tester who owns a camera that pans, tilts or zooms. None of this has ever run against
one: every part is covered by tests against fakes and a local server, which proves the app does
what it intends and proves nothing about what a console does when asked.

Please work through it in order and say what happened, including the parts that worked. "Preset 3
did nothing and said nothing" is as useful as an error message, and more useful than silence.

**What you need.** A PTZ camera adopted in Protect, at least two saved positions in the Protect
app, and Camural connected to that console. Some parts need a console sign-in, which is optional
and covered in part 3.

---

## 1. Presets, with only an API key

1. Open Settings and tick **PTZ** for your PTZ camera. Nothing should change anywhere else.
2. Right-click that camera's window on your desktop. There should be a **Point camera** submenu
   holding Home and Preset 1 to 9.
3. Press **Home**. Watch the camera, not the screen: does it move?
4. Press a preset you have actually saved in Protect. Does it go there?
5. Press a preset you have NOT saved, a high number. You should get a notification saying this
   camera has no preset N saved. Anything else, especially silence, is a finding.
6. Open the camera full size from the tray. There should be a row of buttons along the bottom.
   Press one. Then press the number keys 1 to 9 and Home, including with F11 full screen on, where
   the buttons are hidden.

**The thing most likely to be wrong here** is that Home does nothing while the numbered presets
work. If your Windows is set to Swedish, Finnish, Lithuanian, Norwegian or Estonian, that is a
specific bug this was built to avoid, and finding it anyway is worth knowing.

## 2. One question that needs a console, not the app

This decides a design choice and takes a minute. In a terminal, with your own console address and
API key:

```bash
curl -sk -H "X-API-KEY: YOUR_KEY" "https://YOUR_CONSOLE/proxy/protect/integration/v1/cameras" | head -c 2000
```

In what comes back, is there a `"type"` field on each camera, and does it name the model, for
example `UVC G6 PTZ`? Copy the first camera's worth of that output into your reply, with the id
and mac removed if you would rather.

If it is there, Camural can work out which cameras are PTZ without anybody ticking a box.

## 3. With a console sign-in

> **Parts of this are already done, so do not spend time on them.** On 24 August 2026 a local
> console account was created by following the app's own instructions, signed in against Protect
> 7.2.105, and Camural read the camera list from it without ever being asked for a second factor.
> Steps 1, 3 and the sign-in half of step 4 are proven. What nobody has done is point a camera:
> that console has only fixed cameras and correctly reported none that move. Concentrate on the
> labels in step 2 and on everything from step 5 down.


Optional, and only if you are comfortable creating an account on your own console.

1. In UniFi, add an admin. Tick **Restrict to Local Access Only** and give it local credentials.
   Do not use your Ubiquiti account, which needs a code from your phone at every sign-in.
2. Give it access to the PTZ cameras, with the right to change those cameras' settings, which is
   what write access means in Protect. **Full Management** on those cameras always works.

   **Read the labels on that screen and tell us exactly what they say.** This is the one step
   nobody has verified on hardware. We believe Protect has no PTZ permission at all and that
   moving a camera rides on the same write access that changes its settings, but that is a
   negative claim from three agreeing sources and no official documentation, and nobody has
   opened the page. If you see anything resembling a PTZ toggle, that changes the setup advice
   in the app and on the site. Note your Protect and UniFi OS versions either way.
3. In Camural, edit the console, expand **PTZ camera control**, and enter that username and
   password.
4. Restart Camural. Within a minute or so:
   - Does the PTZ tick stop mattering, so a camera you never ticked also shows controls?
   - Do the buttons carry the names you gave your positions in Protect, instead of Preset 1 to 9?
5. Point the camera somewhere new, then right-click its window and choose **Save this view**. Give
   it a name. Does it appear as a button, and does it appear in the Protect app too?
6. Change the console password in UniFi, without telling Camural. Press a preset. Camural should
   say the sign-in was refused and stop trying. It must NOT keep retrying: if your console starts
   refusing sign-ins from that machine afterwards, that is the most important bug on this page.

## 4. The controller

Needs the sign-in from part 3 and an Xbox controller or anything Windows treats as one.

1. Open the camera full size and click into the window.
2. Left stick pans and tilts. Triggers zoom in and out. A, B, X and Y go to your first four
   positions, holding LB shifts them to the next four, and Start goes home.
3. Click on another window so Camural loses focus, and push the stick. Nothing should move.
4. While pushing the stick, unplug the controller. The camera should stop where it is, not carry
   on turning.
5. Does it feel usable? Too fast, too slow, drifting when you are not touching it? Settings has a
   **Game controller** section beside Your settings with speed, dead zone and invert tilt.

## 5. Patrol

Read this before trying it: a patrol keeps the camera moving until you stop it, and some PTZ
motors are not built for that. Try it briefly rather than leaving it running.

1. Right-click the camera, **Point camera**, **Start patrol 1**. You should get a notification
   saying what a patrol costs, and the tray should say the camera is patrolling and that its
   motion alerts are paused.
2. Does the camera actually patrol?
3. **Stop patrol** should be the only patrol entry in the menu while one is running.
4. Quit Camural while a patrol is running, then start it again. Expected, and worth confirming:
   the camera keeps patrolling because the console is running it, and Camural no longer knows, so
   the menu offers to start one rather than to stop it. Stop it in Protect.

## 6. Alerts, which is the part that must not go wrong

1. With alerts on for the PTZ camera, press a preset and watch for a notification over the next
   fifteen seconds. You should NOT get one about the camera's own movement.
2. During that same window, walk in front of the camera. You SHOULD get a notification about a
   person.
3. Ring the doorbell, if that camera is a doorbell, or trigger something the camera classifies.
   That should come through too.

Anything that alerts on the camera's own panning, or any real event that goes missing, is worth
reporting immediately rather than at the end.

---

## What to send back

The version from the tray menu, your Protect and UniFi OS versions, the camera model, and what
happened at each step. If something failed, the log helps: `%LOCALAPPDATA%\Camural\logs`, one file
per day. It contains camera names and event times, and never your API key, your password or your
video.

<!-- Published for beta testers. The working copy lives in the source repository at
     docs/notes/ptz-tester-checklist.md; edit it there rather than here. -->

# PTZ: what to check on real hardware

For a beta tester who owns a camera that pans, tilts or zooms. **Written against Camural 1.0.18.**

The first round of this ran on 24 August 2026 against four G6 PTZ cameras on Protect 7.2.105 and a
G5 PTZ on a second console, and most of it passed: presets, Home, the empty-preset notification,
the console sign-in, preset names, Save this view, patrol, and the alert hold while a camera
moves. What it found has been fixed, and the parts it changed are marked below.

So this round is narrower. The sections still worth your time are patrol, the controller, which
nobody has touched at all, and anything on hardware unlike the above.

Please work through it in order and say what happened, including the parts that worked. "Preset 3
did nothing and said nothing" is as useful as an error message, and more useful than silence.

**What you need.** A PTZ camera adopted in Protect, at least two saved positions in the Protect
app, and Camural connected to that console. Some parts need a console sign-in, which is optional
and covered in part 3.

---

## 1. Presets, with only an API key

> **All of this passed on 24 August.** Home, all nine presets, and the empty-preset notification.
> Run it as a sanity check on your own hardware rather than as an investigation.

1. Right-click that camera's window on your desktop. On 1.0.18 you should not need the PTZ tick
   in Settings at all.
2. There should be a **Point camera** submenu holding Home and Preset 1 to 9.
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

## 2. Does the tick box still matter?

> **Answered, and acted on.** A tester dumped their camera list and the model name is there:
> `"type":"UVC G6 PTZ"` sitting next to `"type":"UVC G6 180"`. So from 1.0.18 Camural reads it and
> works out which cameras have motors by itself, and the PTZ tick in Settings is an override for
> the ones it misses rather than something everybody has to find.

Worth thirty seconds to confirm on your hardware:

1. Make sure the PTZ tick in Settings is OFF for your PTZ camera.
2. Right-click that camera on the desktop. **Point camera** should be there anyway.
3. Check a fixed camera has not sprouted one.

If a PTZ camera of yours needs the tick, tell me its model name exactly as Protect writes it. That
is a camera the model-name test does not recognise, and the name is the fix.

## 3. With a console sign-in

> **This section is done, and you can skip it unless something misbehaves.** A local console
> account signs in against Protect 7.2.105 without ever being asked for a second factor, preset
> buttons carry the names given in Protect, and Save this view writes a preset that then appears
> in the Protect app. All confirmed on hardware.
>
> One thing changed since: changing the console password used to drop Camural silently back to the
> API key, with the preset names quietly reverting to Preset 1 to 9 as the only clue. It now says
> which console refused it. If you want to check one thing here, check that.


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

> **Nobody has tested any of this.** It is the largest untouched part of the feature.

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

> **This changed in 1.0.18 and is the main thing to look at.** Camural used to offer five patrol
> slots on every PTZ camera whether or not they existed, because an API key alone cannot ask the
> console what is there. One tester's G5 PTZ answered two of them with an error, which is the
> console correctly refusing a patrol that was never saved. Two changes: with a console sign-in,
> the menu now lists only the patrols that exist and calls them by the names you gave them in
> Protect; and a refusal now says the camera has no patrol saved in that slot rather than quoting
> "400 Bad Request" at you.

Read this before trying it: a patrol keeps the camera moving until you stop it, and some PTZ
motors are not built for that. Try it briefly rather than leaving it running.

**First, in the Protect app, look at how many patrols that camera has saved, and what they are
called.** Everything below is measured against that, and without it a wrong menu and a correct
menu look the same.

1. With a console sign-in configured, right-click the camera and open **Point camera**. The patrol
   entries should match what Protect shows: the same number of them, under the same names. Too
   many, too few, or numbers instead of names is the finding.
2. Start one. You should get a notification saying what a patrol costs, and the tray should say
   the camera is patrolling and that its motion alerts are paused.
3. Does the camera actually patrol?
4. **Stop patrol** should be the only patrol entry in the menu while one is running.
5. Now remove the console sign-in, leaving only the API key, and look at the menu again. Camural
   cannot ask what exists on this path, so it falls back to five numbered slots. Press one you
   know is empty: it should say this camera has no patrol saved there. Anything mentioning 400 or
   Bad Request is a finding.
6. Quit Camural while a patrol is running, then start it again. Expected, and worth confirming:
   the camera keeps patrolling because the console is running it, and Camural no longer knows, so
   the menu offers to start one rather than to stop it. Stop it in Protect.

## 6. Alerts, which is the part that must not go wrong

> **Step 1 passed on 24 August**, so a camera moving no longer alerts on itself. Steps 2 and 3
> were not reached, and they are the half that matters more: the hold is only correct if REAL
> events still get through while it is on.

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
happened at each step. Quote any notification you get word for word: several of the changes in
1.0.18 were to the wording of refusals, and the exact sentence says which path produced it. If something failed, the log helps: `%LOCALAPPDATA%\Camural\logs`, one file
per day. It contains camera names and event times, and never your API key, your password or your
video.

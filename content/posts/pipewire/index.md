---
date: '2025-09-17T23:27:36+05:00'
title: 'Fixing PipeWire Lock Issues on Arch Linux: Why Video & Audio Playback Broke'
categories: ["Linux"]
tags: [ "Audio", "Pipewire"]
draft: false
---

# Fixing PipeWire Lock Issues on Arch Linux: Why Video & Audio Playback Broke

The other day, I ran into a frustrating issue on my Arch Linux setup:  
**no video would play anywhere** , not in Chrome, not in Firefox, and not even locally with `mpv`. The files loaded, but playback simply wouldn’t start.  

Digging deeper revealed that the culprit wasn’t the browser or the media player at all, but **PipeWire**, the modern Linux audio and video server.

---

## Diagnosing the Problem

First stop: checking the PipeWire service.

```bash
systemctl --user status pipewire
```

Output:

```
× pipewire.service - PipeWire Multimedia Service
     Loaded: loaded (/usr/lib/systemd/user/pipewire.service; enabled)
     Active: failed (Result: exit-code)
   Main PID: 9386 (code=exited, status=245/KSM)

Sep 17 23:12:53 archlinux systemd[762]: pipewire.service: Failed with result 'exit-code'.
Sep 17 23:12:53 archlinux systemd[762]: Failed to start PipeWire Multimedia Service.
```

So PipeWire wasn’t starting. To get more detail, I ran:

```bash
pipewire -v
```

This showed the key error:

```
[E] unable to lock lockfile '/run/user/1000/pipewire-0.lock': Resource temporarily unavailable
[E] failed to create context: Resource temporarily unavailable
```

Translation: **PipeWire thought it was already running** because the lock file was still in place, even though the process was broken.  

This happens if PipeWire crashes, hangs during suspend, or leaves behind a stale lock file.

---

## The Quick Fix

To recover, I killed all PipeWire-related processes:

```bash
killall -9 pipewire
killall -9 pipewire-pulse
killall -9 wireplumber
```

Then restarted the services:

```bash
systemctl --user restart pipewire pipewire-pulse wireplumber
```

Instantly, video and audio playback started working again.

---

## Why the `-9` Flag?

Normally, `killall` sends **SIGTERM (15)**, which asks a process to exit cleanly. But when a process is stuck, it can ignore this signal.  

`killall -9` sends **SIGKILL (9)**, which forces the kernel to immediately terminate the process. It skips all cleanup, but it guarantees the process is gone.  

In my case, this was necessary because PipeWire was in a bad state and wouldn’t shut down otherwise.

---

## Preventing the Issue

If you run into this again, here’s a safe recovery sequence:

```bash
# First try a graceful stop
killall pipewire pipewire-pulse wireplumber

# If that fails, force it
killall -9 pipewire pipewire-pulse wireplumber

# Clean up stale lock files (if any remain)
rm -f /run/user/1000/pipewire-*.lock

# Restart services
systemctl --user restart pipewire pipewire-pulse wireplumber
```

Also, make sure you don’t have **PulseAudio** installed alongside PipeWire, as they can conflict:

```bash
pacman -Qs pulseaudio
```

Remove if needed:

```bash
sudo pacman -Rns pulseaudio
```

---

## Takeaway

When video or audio playback suddenly stops working on Linux, the problem isn’t always your browser or media player, it might be PipeWire itself.  

The root cause in this case was a **stale lock file** that tricked PipeWire into thinking it was already running. The fix was as simple as killing the stuck processes and restarting the services.  

Now I’ve got a little one-liner script handy in case it ever happens again, but hopefully, PipeWire behaves better going forward!


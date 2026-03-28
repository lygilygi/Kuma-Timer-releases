# KUMA TIMER
**The Bold & Alert Clock System** · Version 1.4.4

> A professional countdown timer for live events, conferences, and broadcast — with OSC control, Companion integration, and a full-screen projector output.

## Download

👉 **[Latest Release](https://github.com/lygilygi/Kuma-Timer-releases/releases/latest)**

| Platform | File |
|---|---|
| macOS Apple Silicon (M1/M2/M3/M4) | `KUMA-Timer-macOS-arm64.zip` |
| macOS Intel | `KUMA-Timer-macOS-intel.zip` |
| Windows 10/11 (64-bit) | `KUMA-Timer-Setup-Windows.exe` |

**macOS:** unzip and drag `KUMA Timer.app` to Applications.
**Windows:** run the installer — KUMA Timer will appear in the Start Menu.

---

# User Guide

## Overview

KUMA Timer has two windows:
- **Control Window** — your operator view (this window)
- **Display Window** — the output sent to a projector or second screen

---

## Timer Controls

| Button | Action |
|---|---|
| **START** | Start the timer from the loaded time |
| **PAUSE** | Pause the running timer |
| **RESUME** | Resume after pause |
| **RESET** | Stop and clear the timer to zero |
| **HIDE TIMER** | Blank the display window (timer keeps running internally) |
| **SHOW TIMER** | Restore the display |
| **-1m / +1m** | Subtract or add 60 seconds while timer is running |

---

## Setting the Time

Use the **H / M / S** spinboxes to enter the desired duration, then click **SET** to load it without starting.

- In **MM:SS** mode the H field is hidden
- In **HH:MM:SS** mode (set in Settings → Display) the H field appears

The status label above the controls shows the current state: `STANDBY`, `LIVE`, `PAUSED`, or the active speaker name.

---

## Presets

Six quick-load buttons (e.g. `5M`, `10M`, `20M`…).

| Interaction | Action |
|---|---|
| **Click** | Load that duration and set the manual input fields |
| **Hold (0.6s)** | Edit the preset value — enter new minutes in the dialog |

Presets are saved automatically to config.

---

## Cuesheet (Runsheet / Sidebar)

The left column is a runsheet of named speakers with individual times.

| Interaction | Action |
|---|---|
| **Double-click a cue** | Load that speaker's time and name into the timer |
| **Drag & drop** | Reorder cues — order is saved automatically |
| **+ button** | Add a new cue (name + minutes + seconds) |
| **− button** | Delete the selected cue |
| **Clear button** | Remove all cues (confirmation required) |

The sidebar can be hidden in **Settings → Display → Show Runsheet**.

---

## Display Modes

Use the **TIMER / CLOCK** toggle in the control panel:

| Mode | Display shows |
|---|---|
| **TIMER** | Countdown (or count-up in overtime) |
| **CLOCK** | Current wall clock time (HH:mm:ss) |

---

## Screen Selection

The dropdown at the top of the control panel lists all connected screens. Select a screen to move the display window there. On a second screen it goes fullscreen automatically; on the primary screen it opens as a regular window.

---

## Time Warp

Time Warp adjusts the timer's speed so it reaches zero at a target moment — without changing the displayed value.

Click **TIME WARP SETTINGS ▲** to expand the panel.

### Duration Warp
Enter how many real minutes/seconds remain until the end of the session. KUMA recalculates the tick speed so the timer expires exactly then.

### Clock Time Warp
Enter the wall clock time at which the timer should reach zero. KUMA calculates the difference from now and adjusts speed accordingly.

| Button | Action |
|---|---|
| **APPLY** | Activate warp — both APPLY buttons are replaced by CANCEL WARP |
| **CANCEL WARP** | Restore normal 1-second tick speed |

> Time Warp only works while the timer is running and has time remaining.

---

## Overtime

When the timer reaches zero:

| Setting | Behaviour |
|---|---|
| **Count Up** | Timer continues counting up with a − prefix and red color |
| **Stay at 00:00** | Timer freezes at zero |
| **Stop Timer** | Timer stops completely |

Additional overtime options in **Settings → Overtime**:
- Change background color when overtime starts
- Blink the clock
- Show a custom on-screen message (e.g. `PLEASE WRAP UP`)
- Send an OSC trigger to Companion

---

## Settings

Open with the **⚙ cogwheel** button at the bottom left.

### Display
- Clock format: **MM:SS** or **HH:MM:SS**
- Background and text colors
- Progress bar and color warnings (orange at N min, red at N min)
- Show/hide sidebar

### Network (OSC)
- Enable OSC protocol
- **Listen port** (default 8000) — Companion sends commands here
- **Companion IP + Port** (default 127.0.0.1 : 12321) — KUMA sends feedback here
- Enable **Web Mirror** server and set its port

### Integrations
- **Interspace Ind (CDEther)** — enable and set device IP; use RESCAN to auto-detect on the local network
- **Dsan Limitimer** — enable and select the serial port

### Overtime
- Behavior, message, background color, blink, OSC trigger path

---

## OSC Reference

### Incoming — Companion → KUMA
Default listen port: **8000**

| OSC Address | Argument | Action |
|---|---|---|
| `/kuma/start` | — | Start timer |
| `/kuma/pause` | — | Pause / resume toggle |
| `/kuma/reset` | — | Reset timer |
| `/kuma/hide` | — | Toggle display visibility |
| `/kuma/time/add` | — | Add 1 minute |
| `/kuma/time/sub` | — | Subtract 1 minute |
| `/kuma/warp` | `int` seconds | Load specific duration (e.g. `300` = 5:00) |
| `/kuma/load` | `int` seconds | Alias for `/kuma/warp` |
| `/kuma/preset` | `int` index (0–5) | Load preset by index (0 = first button) |
| `/kuma/cue` | `int` index | Load cue from runsheet by index |

### Outgoing — KUMA → Companion
Default target: **127.0.0.1 : 12321**

| OSC Address | Value | Sent when |
|---|---|---|
| `/kuma/timer` | `string` e.g. `"04:32"` | Every 500 ms — current displayed time |
| `/kuma/status` | `"LIVE"` | Timer started or resumed |
| `/kuma/status` | `"PAUSED"` | Timer paused |
| `/kuma/status` | `"STANDBY"` | Timer reset or time loaded |
| `/kuma/status` | `"HIDDEN"` | Display window hidden |
| `/kuma/overtime` | `int` `1` | On overtime start *(path configurable)* |

---

## Web Mirror

When enabled (Settings → Network → Enable Web Mirror Server), KUMA runs a local HTTP server. Open `http://localhost:<port>` in any browser on the same network to see the timer — useful for confidence monitors or tablets.

---

## Status Bar

The bottom bar shows:
- **⚙** — opens Settings
- Integration status: `CDEther Connected/Disconnected`, `Limitimer: Connected/Disconnected`
- Local IP address (moves to second line when both integrations are active)
- ☕ Buy me a coffee link

---

## Tips

- **Double-click a cue** to instantly load a speaker — no typing needed
- **Hold a preset button** to change its value without opening Settings
- **HIDE TIMER** keeps the timer running internally — useful during breaks
- Time Warp is transparent to the audience — the display always shows the original countdown value
- OSC feedback (`/kuma/status`) lets Companion buttons reflect the real timer state with no polling

---

*KUMA Timer · © 2026 Pawel Lygan | PL TECH Ltd*

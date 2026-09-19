# Mouse God Mode

**Turn the extra buttons on your mouse into a control layer for Windows.**

Mouse God Mode is an [AutoHotkey v2](https://www.autohotkey.com/) script that turns a mouse with side buttons and a horizontal scroll wheel into a compact window-management controller.

Instead of assigning one action to each extra mouse button, it treats them as **modifier layers**:

- hold the **Forward button** to control windows;
- hold the **Back button** to control workspaces and layouts;
- hold **Right Click** to control tabs;
- click the side buttons normally and they still work as **Back / Forward**.

The result is a surprisingly large set of controls without adding a dozen arbitrary keyboard shortcuts.

If you use a mouse such as an MX Master, work with lots of tabs, or run a tiling window manager such as GlazeWM, this is basically **a second keyboard under your mouse hand**.

---

## Why?

A modern mouse can have:

- left / right / middle click
- vertical scrolling
- horizontal scrolling
- Back
- Forward

Yet Windows usually treats most of those as isolated buttons.

That leaves a lot of possible input combinations unused.

Mouse God Mode turns:

```text
Side button + scroll
Side button + click
Right click + scroll
```

into contextual commands.

So instead of moving your cursor toward tiny UI targets or moving your hand back to the keyboard, common actions become small mouse gestures.

```text
Forward + Wheel ↓       Switch window
Forward + Wheel →       Close window

Back + Wheel ↑          Previous workspace
Back + Wheel →          Move tiled window

Right Click + Wheel ↓   Next tab
Right Click + Wheel →   Close tab
```

The idea is simple:

> **Use buttons as layers, not just buttons.**

---

# Controls

## Forward button — Window layer

Hold the mouse **Forward** button (`XButton2`).

| Gesture | Action |
| --- | --- |
| `Forward + Left Click / Drag` | Alt + Left Mouse Button |
| `Forward + Right Click / Drag` | Alt + Right Mouse Button |
| `Forward + Wheel Up` | Previous window |
| `Forward + Wheel Down` | Next window |
| `Forward + Wheel Left` | Maximize active window |
| `Forward + Wheel Right` | Close active window |
| `Forward + Middle Click` | Restore / Minimize active window |
| `Forward` | Normal browser Forward |

The drag combinations are especially useful when your window manager binds **Alt + mouse dragging** to window movement/resizing.

The side button still behaves normally when pressed by itself.

---

## Back button — Workspace layer

Hold the mouse **Back** button (`XButton1`).

| Gesture | Action sent by default |
| --- | --- |
| `Back + Wheel Up` | `Alt + S` |
| `Back + Wheel Down` | `Alt + A` |
| `Back + Wheel Left` | `Alt + Shift + Left` |
| `Back + Wheel Right` | `Alt + Shift + Right` |
| `Back + Middle Click` | Refresh active application |
| `Back` | Normal browser Back |

I use these with **GlazeWM** for workspace and tiling-window operations.

They are intentionally just normal keyboard shortcuts sent by AHK, so you can change them to match **your own window manager configuration**.

For example:

```ahk
XButton1 & WheelUp::Send "!s"
XButton1 & WheelDown::Send "!a"

XButton1 & WheelLeft::Send "!+{Left}"
XButton1 & WheelRight::Send "!+{Right}"
```

Replace those shortcuts with whatever your setup uses.

---

# Right Click becomes a Tab modifier

This is probably the feature that changes everyday browsing the most.

Inside supported applications, **hold Right Click and use the wheel**:

| Gesture | Action |
| --- | --- |
| `Right Click + Wheel Up` | Previous tab |
| `Right Click + Wheel Down` | Next tab |
| `Right Click + Wheel Left` | New tab |
| `Right Click + Wheel Right` | Close tab |
| Normal Right Click | Normal context menu |

That last part is important.

The script waits to see whether you actually use the wheel while holding Right Click. If you don't, releasing Right Click produces an ordinary right click.

So the same button can act as both:

```text
Right Click             → context menu

Right Click + Wheel     → tab controls
```

without needing a dedicated "mode switch".

---

## Supported Tab applications

The current configuration enables Right-Click Tab Mode in:

```text
Firefox
Microsoft Edge
Google Chrome
Zen Browser
File Explorer
Visual Studio Code
Obsidian
Adobe Acrobat
Windows Terminal
ChatGPT
Vitis IDE
QQ
Spotify
```

This list is easy to change.

Look for:

```ahk
GroupAdd "RButtonApps", ...
```

and add or remove applications.

For example:

```ahk
GroupAdd "RButtonApps", "ahk_exe firefox.exe"
GroupAdd "RButtonApps", "ahk_exe Code.exe"
GroupAdd "RButtonApps", "ahk_exe zen.exe"
```

---

# QQ-specific controls

QQ gets its own Right-Click layer because regular `Ctrl + Tab` behavior is less useful there.

While QQ is active:

| Gesture | Action |
| --- | --- |
| `Right Click + Wheel Up` | `Ctrl + Up` |
| `Right Click + Wheel Down` | `Ctrl + Down` |
| `Right Click + Wheel Left` | `Ctrl + F` |
| `Right Click + Wheel Right` | `Ctrl + H` |

You can remove or modify this section if you don't use QQ.

---

# Application launcher

The script also contains a small **focus-or-launch** application manager.

Pressing a shortcut:

1. focuses the application if it is already open;
2. restores it if its window is hidden/minimized;
3. launches it if it isn't running.

Current bindings:

| Shortcut | Application |
| --- | --- |
| `Win + Z` | Zen Browser |
| `Win + Q` | QQ |
| `Win + T` | Windows Terminal |
| `Win + S` | Spotify |

Add `Shift` to close the corresponding application:

```text
Win + Shift + Z     Close Zen
Win + Shift + Q     Close QQ
Win + Shift + T     Close Windows Terminal
Win + Shift + S     Close Spotify
```

---

# Installation

## Requirements

- Windows 10 / 11
- [AutoHotkey v2](https://www.autohotkey.com/)
- a mouse with `XButton1` / `XButton2`

Recommended:

- a mouse with horizontal-wheel input (`WheelLeft` / `WheelRight`)
- GlazeWM or another configurable window manager

A four-way scroll wheel is not strictly required, but it unlocks most of the interesting combinations.

---

## 1. Download the script

Clone the repository:

```powershell
git clone https://github.com/ZBOX05/mouse-godmode-ahk.git
cd mouse-godmode-ahk
```

Or just download:

```text
ZBOX_RB_XB.ahk
```

from GitHub.

---

## 2. Install AutoHotkey v2

Install AutoHotkey **v2**, not v1:

https://www.autohotkey.com/

The script starts with:

```ahk
#Requires AutoHotkey v2.0
```

so incompatible AutoHotkey versions should be rejected instead of silently running incorrectly.

---

## 3. Configure application paths

The application launcher currently contains paths from my own PC.

For example:

```ahk
ZenPath := "E:\Application\Zen Browser\zen.exe"
QQPath := "E:\Application\Tencent\QQNT\QQ.exe"
```

Change these to match your machine.

Windows Terminal and Spotify are launched through their Windows app identifiers and may work without modification:

```ahk
WTPath := "explorer.exe shell:AppsFolder\Microsoft.WindowsTerminal_8wekyb3d8bbwe!App"

SpotifyPath := "explorer.exe shell:AppsFolder\SpotifyAB.SpotifyMusic_zpdnekdrzrea0!Spotify"
```

If you don't use the application launcher at all, you can simply remove or comment out those bindings.

---

## 4. Configure your workspace shortcuts

The Back-button layer currently assumes these shortcuts exist:

```text
Alt + S
Alt + A
Alt + Shift + Left
Alt + Shift + Right
```

They are part of my GlazeWM workflow, not requirements imposed by Mouse God Mode.

Change:

```ahk
XButton1 & WheelUp::
XButton1 & WheelDown::
XButton1 & WheelLeft::
XButton1 & WheelRight::
```

to whatever shortcuts your window manager uses.

---

## 5. Run it

Double-click:

```text
ZBOX_RB_XB.ahk
```

The script automatically requests administrator privileges.

There is intentionally **no tray icon**:

```ahk
#NoTrayIcon
```

so once everything is configured, it stays out of the way.

---

# Emergency controls

Because the tray icon is hidden, there are two keyboard shortcuts you should remember.

### Reload

```text
Ctrl + Shift + F5
```

Useful after editing the script.

### Exit

```text
Ctrl + Shift + F12
```

Immediately closes Mouse God Mode.

If you're experimenting with your own mappings, remember this one.

---

# Scroll throttling

Mouse wheels can generate input extremely quickly.

Turning every wheel event directly into something like `Ctrl + Tab` can result in one physical flick jumping through many tabs or workspaces.

Mouse God Mode therefore applies a small debounce / rate limit to the relevant gestures:

```ahk
WheelDelay := 40
```

The value is in milliseconds.

If scrolling feels too slow:

```ahk
WheelDelay := 20
```

If one wheel movement occasionally triggers too many actions:

```ahk
WheelDelay := 60
```

The `Alt + Tab` layer is intentionally left unthrottled because Windows already handles its switching rhythm differently.

---

# Running at startup

Once you've tested your configuration, you can launch the script automatically with Windows.

Press:

```text
Win + R
```

and open:

```text
shell:startup
```

Then place a shortcut to `ZBOX_RB_XB.ahk` inside that folder.

Now the mouse layers will be available after login.

---

# Customizing it

This project is deliberately just one readable `.ahk` file.

There is no framework, daemon, configuration database, Electron UI, or background service.

Want another application in Tab Mode?

```ahk
GroupAdd "RButtonApps", "ahk_exe YourApp.exe"
```

Want another launcher?

```ahk
#e::ManageApp("ahk_exe YourApp.exe", YourAppPath, "Open")
+#e::ManageApp("ahk_exe YourApp.exe", YourAppPath, "Close")
```

Want a completely different action?

Change the corresponding gesture.

For example:

```ahk
XButton2 & WheelRight::WinClose "A"
```

could become practically any AutoHotkey command.

That's the point of the project: **the included mapping is useful on its own, but the control-layer idea is more important than my exact bindings.**

---

# Design philosophy

Mouse God Mode follows a few rules.

### Preserve normal mouse behavior

Side buttons still perform Back / Forward.

Right Click still produces a normal Right Click.

The extra functionality appears only when buttons are used as modifiers.

### Use spatially memorable gestures

```text
Wheel Left / Right     → lateral actions
Wheel Up / Down        → cycling / navigation
Forward button         → windows
Back button            → workspace/layout
Right button           → content inside a window
```

This makes the mappings easier to remember than a random collection of global hotkeys.

### Keep frequently repeated actions under one hand

Switch window.

Switch workspace.

Move a tiled window.

Change tab.

Open tab.

Close tab.

Close window.

Maximize window.

Refresh.

These are small operations individually, but they're performed hundreds of times during normal desktop use.

The project exists to make those operations feel like part of the mouse itself.

---

# Who is this for?

You'll probably get the most out of Mouse God Mode if you:

- use a mouse with several extra inputs;
- keep many browser/editor/terminal tabs open;
- use multiple workspaces;
- use GlazeWM or another tiling window manager;
- prefer keyboard-driven workflows but don't want every action to require your keyboard hand;
- like modifying your tools instead of accepting their default controls.

It is especially nice for the strange middle ground where you're **too keyboard-heavy for normal Windows controls, but still use the mouse constantly**.

---

# Current limitations

This is currently my personal configuration published as a reusable script, rather than a polished end-user application.

In particular:

- some application paths are specific to my machine;
- workspace controls assume my GlazeWM shortcuts;
- applications may use different tab shortcuts;
- horizontal wheel gestures require compatible mouse hardware/software;
- the script requests administrator privileges;
- there is currently no GUI configuration screen.

If your workflow is different, edit the mappings.

The entire implementation is small enough that this is usually easier than adding a large configuration system.

---

# Contributing

Ideas, bug reports and pull requests are welcome.

Good additions would include:

- mappings for more applications;
- cleaner application detection;
- alternative window-manager presets;
- mouse compatibility notes;
- better configuration without sacrificing the simplicity of the script.

If you've built an interesting control layer around the same idea, I'd also like to see it.

---

## One mouse. Three control layers.

```text
Forward + ...       Windows
Back + ...          Workspaces
Right Click + ...   Tabs
```

No extra hardware.

No resident utility suite.

No complicated UI.

Just AutoHotkey and a mouse that was capable of doing far more than Windows normally lets it do.

**Give the unused half of your mouse something useful to do.**

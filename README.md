# Windows Setup Notes

Things I set up on a fresh Windows install. Personal log — not a guide.

---

## 0. First things first

Get a proper manly wallpaper on there. Delete all desktop icons.

![Peepo Windows wallpaper](images/pepe-window.webp)

Everything else builds on this.

---

## 1. Making Windows 11 bearable

### System Cleanup

[ChrisTitus WinUtil](https://github.com/christitustech/winutil)

```powershell
# ChrisTitus WinUtil — run in PowerShell as Admin
irm christitus.com/win | iex
```

Removes telemetry and other crap. Just the standard selection is fine.

I also go to the **Tweaks** tab → **High Performance** (or **Ultimate Performance** if it shows up).

<img src="images/winutil.png" alt="ChrisTitus WinUtil showing the Tweaks tab with Power Plan options" width="50%">

### Package Management

| Tool | Notes |
|---|---|
| [UniGetUI](https://github.com/Devolutions/UniGetUI) | GUI frontend for Windows package managers. Covers WinGet, Scoop, Chocolatey, pip, npm, .NET Tool, and PowerShell Gallery — all in one place. |

```powershell
winget install --exact --id MartiCliment.UniGetUI --source winget
```

> The export/import feature is what I actually use it for — snapshot the entire installed software list, restore on a fresh machine in one go.

<img src="images/unigetui.png" alt="UniGetUI main interface showing installed packages across multiple package managers" width="50%">

### Window Management

Tiling via **[TileWindows.ahk](https://github.com/CostaFot/autohotkey/blob/main/TileWindows.ahk)** — tiles all open windows into a grid, toggles back to maximised on second press. No dedicated tiling WM needed.

**Setup:**
1. Install [AutoHotkey](https://www.autohotkey.com/)
2. Download [TileWindows.ahk](https://github.com/CostaFot/autohotkey/blob/main/TileWindows.ahk)
3. Double-click to run — `H` appears in the system tray

**Hotkey:** `Ctrl + Alt + T`

![AHK tiling script in action — windows splitting into a grid on Ctrl+Alt+T](images/autotile.gif)

**Grid behaviour:**
- 2 windows → side by side, full height
- 3 windows → 2×2 grid, last window stretches to fill its row
- 4 windows → 2×2 grid, equal
- 5 windows → 3×2, last row fills equally
- n windows → nearest square root grid

**Autostart:** `Win + R` → `shell:startup` → shortcut to the `.ahk` file in that folder.

**Going further: GlazeWM**

[GlazeWM](https://github.com/glzr-io/glazewm) — i3-inspired tiling WM. Auto-tiles as windows open instead of a manual hotkey. I am not the biggest fan of it but hey, it works..

```powershell
winget install glzr-io.GlazeWM
```

### Terminal

Windows Terminal + WSL2 for a real Linux shell. Git Bash when I want something Unixy without spinning up WSL.

```powershell
winget install --id Git.Git -e --source winget
```

### Launchers

| Tool | Notes |
|---|---|
| PowerToys Run (`Alt + Space`) | Enough for 99% of cases. |
| Windows Command Palette | Supposed to be PowerToys Run 2.0. Still needs work. |
| [Flow Launcher](https://www.flowlauncher.com/) | Not bad but PowerToys Run does the job. |
| [Everything + EverythingToolbar](https://www.voidtools.com/) | File search only. Very fast. Bit of an overkill. |

#### PowerToys Run plugins

| Plugin | Notes |
|---|---|
| [Video Downloader](https://github.com/ruslanlap/PowerToysRun-VideoDownloader) | `dl <url>` from PowerToys Run. Supports MP4, MP3, quality selection, subtitles. Powered by yt-dlp. |

`Alt + Space` → `dl https://youtube.com/...` → Enter.

![Video Downloader plugin in PowerToys Run](images/powertoys_run_dl.png)

### Radial Menu

| Tool | Notes |
|---|---|
| [Kando](https://github.com/kando-menu/kando) | Cross-platform pie/radial menu. I open it via a mouse button and navigate by direction. |

Not a keyboard-first tool — built around mouse gestures. If I'm already reaching for the mouse, it's fast. Otherwise hotkeys more efficient.

![Kando pie menu opening and navigating with mouse gestures](images/kando.gif)

---

## 2. WSL2 — Windows Subsystem for Linux

Best thing that happened to Windows in the past 15 years. Real Linux environment, no VM overhead, no dual boot.

- bash, zsh, grep, sed, curl, git, Python, Node etc
- Windows files accessible inside Linux at `/mnt/c/`, Linux files accessible in Explorer via `\\wsl$\`

### Install

```powershell
wsl --install
```

Installs WSL2 + Ubuntu by default. Restart, then finish Ubuntu setup.

### After install

```bash
sudo apt update && sudo apt upgrade -y
```

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

### Accessing files

| What | How |
|---|---|
| Windows files from Linux | `/mnt/c/Users/YourName/` |
| Linux files from Windows | `\\wsl$\Ubuntu\home\yourname\` in Explorer |
| Open current Linux folder in Explorer | `explorer.exe .` from inside WSL |
| Open current Linux folder in VS Code | `code .` from inside WSL |

> I keep project files inside the Linux filesystem (`~/projects/`) rather than under `/mnt/c/` — filesystem performance across the WSL boundary is noticeably slower for file-heavy operations.

---

## 3. Mouse with extra buttons

[Logitech G502 Lightspeed](https://www.logitechg.com/en-gb/shop/p/g502-lightspeed-wireless-gaming-mouse) — 11 programmable buttons. I use it as a mouse-only control surface alongside Kando.

<img src="images/g502.jpg" alt="G502 Lightspeed with button labels annotated" width="50%">

Extra buttons cover common actions without touching the keyboard — pairs well with Kando for launching/switching and AHK hotkeys as fallback.

### Button mapping

| Button | Mapping |
|---|---|
| Scroll wheel tilt left | Kando/radial menu trigger |
| DPI shift (next to left click) | Volume up/down |
| G4 / G5 (thumb buttons) | Shift+Alt+Tab / tile windows (TileWindows.ahk) |

### Software

| Tool | Notes |
|---|---|
| [Logitech G HUB](https://www.logitechg.com/en-gb/innovation/g-hub.html) | Per-app profiles, button remapping, macro recording, DPI config, RGB. |

---

## 4. Hiding the taskbar

Single screen, no distractions, maximum space. With TileWindows handling layout and Kando on the mouse, the taskbar is dead real estate.

**Taskbar settings → Taskbar behaviours → Automatically hide the taskbar**

![Taskbar settings panel with Automatically hide the taskbar checked](images/taskbar.png)

Slides out of view when not in use, reappears on mouse-to-bottom-edge. Notification badges are hidden until then — I don't care about that.

---

## 5. Startup configuration

### Startup folder

For anything without a built-in autostart toggle: `Win + R` → `shell:startup` → drop a shortcut in there.

### What I run on login

| App | How |
|---|---|
| **TileWindows.ahk** | Shortcut to `.ahk` file in `shell:startup` |
| **Kando** | Shortcut in `shell:startup` |
| **PowerToys** | Built-in autostart toggle |
| **Logitech G HUB** | Built-in autostart toggle |
| **LocalSend** | Built-in autostart toggle — Settings → Start hidden |

---

## 6. File sharing between devices

### LocalSend

[LocalSend](https://localsend.org/) — open source AirDrop equivalent. Transfers files between any devices on the same WiFi, no internet, no account, no cloud, no cables. Windows, macOS, Linux, Android, iOS.

```powershell
winget install LocalSend.LocalSend
```

No pairing needed — devices discover each other automatically.

---

## 7. Screenshots

### tinyshots

[tinyshots](https://www.tinyshots.app/web) — browser-based screenshot beautifier. Paste a screenshot, add padding, rounded corners, gradient backgrounds, shadows, device frames. Using it for anything that needs to look presentable.

Workflow: `PRTSC` to capture to clipboard → paste into tinyshots → export.

<img src="images/tinyshot_peepo.png" alt="peepo has never looked this good" width="50%">

---

## 8. Text editing

For config files, scripts, logs — anything that doesn't need a full-blown IDE.

### Notepad++

[Notepad++](https://notepad-plus-plus.org/) — syntax highlighting for everything, tabs, regex find/replace, column editing, macros, plugins. Free, open source, still maintained.

```powershell
winget install Notepad++.Notepad++
```

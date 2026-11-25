# Windows 11 26H2 Taskbar Position Auto-Hide (KB5120998)

[Windhawk](https://windhawk.net/) mod implementing per-monitor edge auto-hide for `Shell_TrayWnd` and `Shell_SecondaryTrayWnd` across **Left, Top, Right, and Bottom** positions on Windows 11 (26H2 / KB5120998+).

Native `ABS_AUTOHIDE` only supports the bottom edge. This mod handles hiding, revealing, and work area retention directly via Win32/DWM APIs without hooking `taskbar.dll` or `Taskbar.View.dll` symbols.

## Implementation
- **Subclassing:** Attaches `TaskbarSubclassProc` to `Shell_TrayWnd` and `Shell_SecondaryTrayWnd` on the taskbar UI thread via `SetWindowSubclassFromAnyThread`.
- **Hide state:** Cloaks the taskbar with `DwmSetWindowAttribute(..., DWMWA_CLOAK, TRUE)` and applies an empty `SetWindowRgn(0, 0, 0, 0)` instead of `SW_HIDE`, keeping the XAML host alive while passing pointer input through.
- **Slide transition:** Moves along the detected monitor edge with `SetWindowPos(..., HWND_TOPMOST, ..., SWP_NOSENDCHANGING)` and clips to monitor bounds via `SetWindowRgn` (ease-out cubic, `10 ms` `USER_TIMER_MINIMUM` tick while moving, `25 ms` idle poll). Suppresses `WM_WINDOWPOSCHANGING` and `WM_WINDOWPOSCHANGED` during frames so `TrayUI` does not revert coordinates.
- **Work area lock:** Hooks `SystemParametersInfoW` and `SystemParametersInfoForDpi` (`SPI_SETWORKAREA`) to keep `rcWork == rcMonitor`, preventing Explorer from resizing maximized or borderless windows when the taskbar reveals.
- **Input & flyouts:** Ignores edge triggers while `VK_LBUTTON` or `VK_RBUTTON` is down. Remains revealed while foreground or pointer is over the taskbar, `#32768`, `Xaml_WindowedPopupClass`, `NotifyIconOverflowWindow`, `TaskListThumbnailWnd`, `StartMenuExperienceHost.exe`, `SearchHost.exe`, `ShellExperienceHost.exe`, or `Widgets.exe`.
- **Native state:** Clears `ABS_AUTOHIDE` (`ABM_SETSTATE`) while active and restores prior state + work area on uninit.

## Install
1. Install [Windhawk](https://windhawk.net/).
2. Open Windhawk → **Explore** → **Create a new mod**.
3. Paste [`win11-26h2-taskbar-position-auto-hide.wh.cpp`](win11-26h2-taskbar-position-auto-hide.wh.cpp).
4. Compile and exit editing mode.

## Settings
| Setting | Default | Description |
| --- | --- | --- |
| `animationDurationMs` | `160` | Slide duration in ms (`0` disables animation). |
| `hoverShowDelayMs` | `40` | Edge dwell time in ms before reveal (`4 px` trigger strip). |
| `hideDelayMs` | `220` | Delay in ms before hiding after pointer leaves taskbar and flyouts. |

## License
[MIT](LICENSE)
## [1.0.0] - 2026-11-26

### Added
- Per-monitor taskbar auto-hide for `Shell_TrayWnd` and `Shell_SecondaryTrayWnd` across **Left, Top, Right, and Bottom** edges on Windows 11 (26H2 / KB5120998+).
- UI-thread subclassing via `WindhawkUtils::SetWindowSubclassFromAnyThread`.
- `DWMWA_CLOAK` + empty `SetWindowRgn(0, 0, 0, 0)` hide state to keep the XAML host alive while passing pointer input through.
- Ease-out cubic edge-slide transition (`10 ms` `USER_TIMER_MINIMUM` step while animating, `25 ms` idle poll) with monitor-clipped `SetWindowRgn` and `WM_WINDOWPOSCHANGING` / `WM_WINDOWPOSCHANGED` suppression during frames.
- `SystemParametersInfoW` and `SystemParametersInfoForDpi` (`SPI_SETWORKAREA`) hooks to hold `rcWork == rcMonitor` and prevent open/maximized/borderless windows from resizing on reveal.
- Pointer-drag suppression (`VK_LBUTTON` / `VK_RBUTTON`) and active flyout detection (`StartMenuExperienceHost.exe`, `SearchHost.exe`, `ShellExperienceHost.exe`, `Widgets.exe`, `#32768`, `Xaml_WindowedPopupClass`, `NotifyIconOverflowWindow`, `TaskListThumbnailWnd`).
- Automatic `ABS_AUTOHIDE` override while active, restoring prior state and work area on uninit.
- Configurable `animationDurationMs`, `hoverShowDelayMs`, and `hideDelayMs` settings.
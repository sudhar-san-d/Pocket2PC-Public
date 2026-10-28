# Changelog

All notable changes to Pocket-to-PC are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.1] - 2026-10-28

### Fixed
- Fixed: update checker now uses the public releases page.

## [2.0.0] - 2026-10-28

### Added
- Redesigned UI with a shared design system (new theme, Inter / JetBrains Mono fonts).
- Android light theme following system settings (dark theme remains default).
- Wake-on-LAN "Wake PC" button in offline state with MAC address guidance.
- GitHub releases update check banner on both Android and PC apps (daily check with silent failure offline).
- Download folder picker via Storage Access Framework (SAF) in Android Files screen.
- "Always allow for this phone" consent option on PC pairing and sensitive action prompts (revocable from Devices).

### Changed
- Android navigation restructured into a 5-tab bottom navigation bar (Home, Remote, Files, Workflows, More), with "More" hosting Logs, Serial Monitor, Devices, and About.
- PC dashboard completely rebuilt with CustomTkinter and a modern left sidebar (Home, Devices, Transfers, Activity, Settings).
- Improved PC system tray menu with direct dashboard double-click action, Pairing QR button, Pause Input toggle, and device revocation.
- Friendlier labels replacing technical jargon across both platforms (e.g., "Secure link" for mTLS, "Device ID" for fingerprint, "Activity" for lanes).

### Fixed
- Touch target sizes across buttons, sub-tabs, and chips meeting minimum 48dp accessibility standards.
- Raised text-secondary contrast to exceed WCAG 4.5:1 standards.
- Fixed text clipping and overflow at large font scales (1.3x+) using dynamic sizing and imePadding on modal dialogs.
- High-DPI blur on Windows displays by setting per-monitor DPI awareness at startup.
- Reverted touchpad input handler to direct sub-pixel mouse movement, eliminating input jitter.

## [1.3.1] - 2026-10-27

### Fixed
- Fixed CI release build failures caused by bare `pytest` invocations on Windows runners by using explicit `python -m <tool>` commands.
- Added comprehensive toolchain pre-flight checks verifying `pytest`, `ruff`, `mypy`, `bandit`, and `pip-audit`.
- Made Android release APK signing gracefully conditional on the presence of `ANDROID_KEYSTORE_*` secrets, allowing Windows installer builds to complete cleanly when keystore secrets are omitted.
- Configured console output encoding to UTF-8 and sanitized release script banner to plain ASCII.
- Added `workflow_dispatch` trigger with `dry_run` support to test the release packaging pipeline without publishing.

## [1.3.0] - 2026-10-27

### Added
- **Workflows, Power & System Controls**:
  - Introduced curated workflow packs: Everyday (`lock_pc`, `screen_off`, `sleep_pc`, `shutdown_pc`) and Web & Files (`open_browser`, `open_downloads`), with Developer and Embedded packs disabled by default.
  - Mandatory confirmation prompts for destructive/state-changing workflows (`lock_pc`, `screen_off`, `sleep_pc`, `shutdown_pc`).
  - Live PC battery card on Home showing battery percentage, charging state (charging / plugged in / on battery), formatted time remaining, dynamic charging bolt icon, and battery health color coding (green charging, amber <= 20%). Automatically hides on systems without batteries (e.g., desktops).
  - Internal laptop display brightness slider using WMI CIM (`WmiMonitorBrightness`), with friendly "not supported" badge for external monitors.
  - Windows Dark mode toggle via Personalization registry keys. (Night Light safely omitted due to Windows 10/11 undocumented proprietary `CloudStore` binary registry blobs).
  - Favorite Sites management: View, add, edit, delete, and reorder favorite websites from Android, stored on PC (`%APPDATA%/PocketToPC/favorites.json`), opening directly in the default browser with strict HTTP/HTTPS validation.
  - Screenshot to Phone: Full-screen screen capture (`mss` with `SetThreadDesktop` context switching), transmission to mobile, preview dialog with Save to `Pictures/PocketToPC` and Share intent, native Windows notification toast, and audit logging.
  - Find File by Name: Fast file search strictly scoped to allowlisted user folders (Desktop, Documents, Downloads, Pictures), skipping hidden/system files, with ranked results, size, and date metadata, downloadable via the file lane.
  - Auto-detection for VS Code across PATH, standard installation folders, and Registry; missing dependencies (like `scrcpy`) now render friendly "not installed" cards instead of failing.
- **Open Link on PC & Share Target**:
  - Android Share-Sheet Target (`ShareTargetActivity`): "Open on PC" share intent receiver for text and links with fast (< 1s) execution and instant toast feedback.
  - Smart URL extraction: Regex parser extracting clean URLs from dirty shared text (such as YouTube share strings with video titles).
  - 15-second reconnect retry window for offline PC hosts, falling back to a "Send as snippet" option for plain text payloads.
  - In-app "Open copied link on PC" button on Home dashboard with haptic feedback.
- **Quick Send & Snippets Integration**:
  - Android `RemoteInput` notification action enabling users to type and send clipboard snippets directly from the system notification shade without foregrounding the app.
  - Dedicated in-app "Quick Send" sheet on Home and Files screens with direct copy-to-clipboard on PC capability (`clipboard.write`).
  - Notification deep link support leading directly to the Files > Snippets tab with instant one-tap copy button.
- **Multiple PC Pairing & Switching**:
  - Multi-PC profile storage on phone (fingerprint, friendly name, endpoint, last seen timestamp).
  - Discovery and fingerprint-based reconnection avoiding broken static IP addresses.
  - PC Switcher bottom sheet with online/offline status indicators, quick switching, friendly renaming, and device revoking.
- **App Indexer & Voice Matching**:
  - Windows Start Menu (`.lnk`), Registry `App Paths`, and Windows Store (`shell:AppsFolder`) app indexer, refreshed on daemon startup and on-demand.
  - Safe "Open <app>" voice execution with fuzzy matching and aliases, strictly restricted to indexed applications.
  - Ambiguity resolution: Top 3 "Did you mean..." suggestions when matching confidence is moderate.
  - Voice URL navigation: "Open <site>" for favorites, "go to <domain>", and "search <query> on Google/YouTube" via templated HTTP/HTTPS URLs.
- **UX & Onboarding Polish**:
  - First-run setup dialog ("What do you use your PC for?") enabling customized workflow packs on initial launch, editable in Settings.
  - Workflows screen enhancements: Pinned favorites row with long-press pin/unpin and reordering; Recent runs row (last 5 runs); friendly names and icons; warning badges replacing verbose risk text; execution result cards ("Done: <summary>").
  - "Add my own" workflow creator wizard for custom apps, websites, and allowlisted folders without manual YAML editing.
  - Haptic ticks on actions, dismissible undo snackbars, and friendly empty states.

### Fixed
- **Plain Text Snippet Naming**: Corrected automatic file naming for received text snippets to guarantee reliable `.txt` extension generation, sanitized base filenames, and collision-free unique timestamps.
- **Trackpad Mouse Movement**: Reverted trackpad cursor movement to smooth 125Hz coalesced pynput controller with relative motion handling, resolving cursor lock and stuttering across high-refresh desktop monitors.
- **Standardized Production Inbox**: Standardized the default received file destination to `~/PocketToPC/Inbox` across all file lane handlers, background services, and audit logging.
- **Connection & State Resiliency**: Resolved race condition in live feed event publishing, enhanced strict URL scheme validation, corrected mute key events, and improved host connection status synchronization.

## [1.2.1] - 2026-10-26

### Fixed
- **Top App Bar Layout & Responsiveness**:
  - Re-architected `AppTopBar` across all screens to ensure title takes `weight(1f)` with `maxLines = 1` and `TextOverflow.Ellipsis`, so text shrinks first on narrow displays without pushing action controls off-screen.
  - Replaced raw verbose `DAEMON_ONLINE` string with a compact status indicator: showing a colored dot with short "Online" / "Offline" label on wide screens/landscape, and dot-only on compact widths or large font scales (`fontScale > 1.2`), with full description in Tooltip and accessibility semantics.
  - Standardized Info and QR actions as fixed-size 40dp IconButtons with 8dp spacing and proper system insets (`statusBars` + `displayCutout`), completely preventing clipping at screen edges.
  - Integrated `AppTopBar` in landscape orientation atop the main content pane for consistent header navigation and unclipped full-length screen titles.

## [1.2.0] - 2026-10-26

### Added
- **Full Landscape Support & Responsive Two-Pane Layouts**:
  - Replaced `NavigationBar` with a compact `NavigationRail` in landscape orientation with full display cutout inset handling.
  - Screen Preview in landscape: Live PC video feed on the left pane, touch canvas and mode toggles on the right pane.
  - Touchpad & Remote Packs in landscape: Large touch surface on the left pane, click/scroll controls and shortcuts on the right pane.
  - Home Dashboard in landscape: Connected host telemetry and Task Manager graphs on the left, bridge utility shortcuts and recent runs on the right.
  - Workflows Launcher in landscape: Category filter pills and searchable workflow list on the left, selected workflow details and execution trigger on the right.
  - Files & Sync in landscape: Master sidebar navigation on the left, active file transfer view on the right.
  - Device Management in landscape: Trusted host parameters and mTLS capabilities on the left, discovered mDNS agents on the right.
  - Logs & Serial Monitor in landscape: Filter and port dropdown controls on the left, live terminal log streams on the right.
  - About Screen in landscape: Product branding and status on the left, permanent author attribution on the right.
  - Pairing QR Dialog: Adaptive side-by-side camera viewport and manual pairing instructions.
  - Zero Connection Drops on Rotation: Non-destructive Activity configuration changes preserving all active WebSockets, live streams, and background transfers during orientation changes.
- **Fullscreen Mode in Direct Touch (Landscape Only)**:
  - One-tap Fullscreen action visible exclusively when the device is in landscape orientation and Direct Touch mode.
  - Immersive edge-to-edge window configuration with hidden system bars and short-edge cutout rendering.
  - Keep-screen-on wake lock during fullscreen preview sessions.
  - Floating HUD overlay featuring Exit, Keyboard toggle, live FPS, Stop stream, and Pause Input kill-switch with 3-second auto-hide and edge swipe re-appearance.
  - System back gesture handling to gracefully exit fullscreen mode.
  - Touch mapping engine mathematically compensating for letterbox/pillarbox margins with coordinate clamping.

### Fixed
- **Screen Preview Mode Toggle Bug**:
  - Restructured preview header into two distinct rows: Row 1 holds the Start/Stop button, connection status, and Fullscreen toggle; Row 2 houses the full-width segmented "Touchpad | Direct touch" mode toggle.
  - Single-line non-wrapping text layout (`maxLines = 1`, `softWrap = false`) preventing label truncation or vertical wrapping across all screen widths and font scales up to 1.3x.
  - Removed duplicate keyboard button from the preview card, standardizing on the primary keyboard action in the header bar.

## [1.1.0] - 2026-10-26

### Added
- **Remote Keyboard Input**: Full keyboard support in Remote tab (both Touchpad and Screen Preview modes) allowing direct typing into any focused PC application after clicking with the touchpad.
  - **Live & Compose Modes**: Toggle between Live typing (character-by-character input with full IME composition, emoji, and Unicode support) and Compose mode (line/paragraph buffer with Send button).
  - **Zero-Width Hidden Field Pattern**: Hidden text field with sentinel character guaranteeing Backspace keystroke capture even when the phone's input field is visually empty.
  - **Slim Special-Keys Bar**: Dedicated quick bar providing Esc, Tab, Enter, Backspace, arrows (←, ↑, ↓, →), Home, End, and one-tap shortcuts for Copy (Ctrl+C), Paste (Ctrl+V), and Select All (Ctrl+A).
  - **Sticky Modifiers**: Toggleable Ctrl, Alt, Shift, and Win modifier states for seamless keyboard combos.
  - **Windows UI Automation Auto-Show**: Automatically detects when an editable text control is focused on Windows and sends a debounced `text_focus` event to open the Android IME.
  - **Password Field Detection & Masking**: Detects password inputs (`CurrentIsPassword` / `ES_PASSWORD`), shows a lock indicator on the phone, and visually masks typed characters.
  - **Unicode & Long Text via Safe Clipboard**: Clipboard-based Ctrl+V injection with automatic original clipboard restoration for multi-character strings, non-ASCII symbols, and emojis.
  - **IME-Aware Layout**: Inset padding (`Modifier.imePadding()`) preserving screen preview and touchpad visibility above the active soft keyboard.

### Security & Privacy
- **Zero Raw Keystroke Logging**: Typed text and keys are strictly stripped from logs, audit trails, and event bus; only event counts and timestamps are recorded.
- **Per-Device Rate Limiting**: Token bucket rate limiter preventing input flooding and DOS while maintaining human typing speeds.
- **Capability Enforcement**: All keyboard operations, focus streams, and settings require valid `Capability.INPUT` authorization and obey the "Pause Input" kill-switch and idle auto-disable.

## [1.0.0] - 2026-10-26

### Added
- **Permanent Author Identity**: Official author branding credited to **Sudharsan** across PC tray, About dialog, Android BuildConfig, and Windows installer metadata.
- **Single-Source Versioning**: Canonical root `VERSION` file driving Windows packaging, installer scripts, and Android releases.
- **Multi-Lane Local Companion Protocol**:
  - **Screen Lane**: Low-latency screen preview and telemetry mirroring.
  - **Input Lane**: Remote touchpad, gesture navigation, and pause input switch.
  - **File Lane**: Fast Wi-Fi file sharing with user confirmation prompt and automatic inbox filing.
  - **Voice & Workflow Lane**: Allowlisted YAML workflow triggers with safe parameter interpolation.
  - **Telemetry & Serial Lane**: Real-time PC performance monitoring and read-only USB serial monitor.
- **Dedicated Pairing QR Viewer**: Built-in, high-contrast QR display window with automatic lifecycle management and on-screen pairing.
- **Single-Instance Windows Service**: Dedicated tray launcher with Dashboard, Settings, Start with Windows toggle, and one-click diagnostics export.
- **Automated CI/CD Release Pipeline**: GitHub Actions release workflow producing verified Windows installers, Android release APKs, and SHA256 checksums on tag pushes.

### Security & Privacy
- **Zero-Trust Local Transport**: 100% offline local communication over Wi-Fi/LAN. Never uses cloud relays or external servers.
- **Hardware-Backed Device Identity**: Android Keystore ECDSA P-256 keys paired with ephemeral per-install PC Certificate Authority.
- **Strict Single-Use Pairing Tokens**: Ephemeral 300-second token lifecycle with automatic rotation and replay attack rejection.
- **Tamper Evidence**: Cryptographic branding integrity guard embedded at build time.

### Fixed
- **Android R8 Obfuscation**: Explicit `@SerialName` annotations across all 35 data transfer models, ensuring reliable JSON serialization in minified release builds.
- **Tkinter Worker Threading Deadlock**: Fixed cross-thread UI freezes during pairing authorization.
- **Dynamic Certificate SAN Validation**: Automatically detects network adapter switches and re-issues TLS certificates matching active IP addresses.

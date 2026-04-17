---
title: macOS
type: product
created: 2026-04-16
updated: 2026-04-17
sources: [macOS安装U盘制作.md, macOS常用命令.md, macOS开发环境配置.md, macOS系统设置.md]
tags: [macos, apple, desktop-os, unix, bsd, command-line, system-administration, networking, diskutil, launchctl, pmset, homebrew, xcode, defaults, recovery, createinstallmedia, bootable-media, startup-security, downgrade, troubleshooting, development-environment, ruby, rbenv, cocoapods, ios, system-settings, keyboard-shortcuts, input-method, trackpad, chrome, ergonomics]
---

# macOS

Apple desktop operating system page covering installer/recovery workflows, interactive desktop preferences, day-to-day command-line administration, and development environment setup across system state, input, trackpad, networking, services, power, and developer tooling.

---

## Overview

In the current knowledge base, macOS is now represented through four complementary operational lenses:

- reinstall / downgrade preparation through installer media, Recovery, and startup-security changes
- everyday desktop ergonomics through shortcut, input-source, trackpad, and browser-behavior preferences
- day-to-day local administration through a mix of BSD/Unix commands and Apple-specific control surfaces
- development environment setup through language version managers and platform-specific toolchains

This makes macOS in this wiki not just a GUI desktop system, but also a developer workstation whose usability depends on both command surfaces and interaction-layer preferences.

## Product Snapshot

| Aspect | Details |
|---|---|
| Product type | Apple desktop operating system and local developer workstation platform |
| Command model | Mix of BSD/Unix tools plus Apple-specific interfaces |
| Interactive preference surfaces | keyboard shortcuts, input-source defaults, trackpad behavior, browser performance toggles |
| Apple-specific interfaces in current sources | `createinstallmedia`, `system_profiler`, `diskutil`, `launchctl`, `pmset`, `scutil`, `defaults`, `socketfilterfw` |
| Developer-tooling layer | `Homebrew`, Xcode Command Line Tools, shell environment variables |
| Daily administration surfaces | system info, storage, networking, processes, services, power, users, clipboard/screenshot/open commands, Finder/Dock defaults, System Settings adjustments |
| Installer distribution | App Store installer apps, with older versions sometimes discovered through web listings |
| Recovery control surface | `Recovery` -> `Startup Security Utility` |

## Interactive Desktop Preferences

The newest macOS source in the wiki is much lighter-weight than the installer and command-line material, but it adds an important workstation layer: everyday settings that reduce friction in typing, pointing, and browser interaction.

### Shortcut and Input Baseline

- The source records a preferred shortcut split between input-source switching and `Spotlight`, plus a recommendation to keep the default input method in English.
- Because the raw note presents these as desired settings rather than a documented OS baseline, the wiki should treat them as preference guidance, not universal macOS defaults.
- This matters for technical writing because Mac setup docs often blur together “what the team recommends” and “what ships by default.”

### Trackpad Behavior as a Productivity Setting

- `Secondary click -> Click in Bottom Right Corner` and enabling `Tap to click` frame the trackpad as part of workstation ergonomics, not just personal taste.
- That gives the wiki a place to capture interaction details that affect day-to-day efficiency but do not belong in command-only references.

### App-Specific Browser Workarounds Also Belong on the Workstation Surface

- The Chrome typing-lag note shows that some “Mac experience” problems are really application-level behaviors.
- In this case, the remedy is to disable webpage preloading rather than to change macOS internals.
- Future workstation docs should therefore distinguish OS-level settings, input-layer settings, and app-level performance toggles.

## Day-to-Day Command-Line Surface

### System Inspection and Storage

- `sw_vers`, `system_profiler`, `sysctl`, `uptime` and `pmset -g batt` provide a quick read on OS version, hardware, runtime, and laptop battery state.
- `df` and `du` handle capacity inspection, while `diskutil list/info/eject` exposes Apple-specific disk and external-media control.
- This makes macOS storage documentation different from generic Unix notes: the filesystem view and the removable-media/device view are both first-class.

### Network, Service, and Process Control

- The current source uses `lsof -i`, `netstat`, `route`, `ifconfig`, `airport`, and `scutil --dns` to inspect ports, routing, interfaces, Wi-Fi, and DNS configuration.
- Firewall status is handled through `socketfilterfw`, while background services are managed through `launchctl`.
- Process control is still recognizably Unix-like (`ps`, `top`, `kill`, `killall`, `pkill`, `nohup`, `jobs`, `fg`), but the service layer is Apple-specific rather than `systemd`.

### Power, Identity, and Desktop Automation

- `pmset`, `caffeinate`, and `shutdown` turn power behavior into a scriptable control surface.
- `dscl`, `groups`, and `su -` expose local identity and user information.
- `pbcopy`, `pbpaste`, `screencapture`, `say`, `open`, and `defaults write` connect terminal work to desktop workflows and Finder/Dock/SystemUIServer behavior.

### Developer Tooling

- `brew` is the main package-management surface in the current source for installing and updating local developer tools.
- `xcode-select` and `xcodebuild -license accept` represent the Apple toolchain side of developer machine setup.
- The source also treats shell environment configuration (`env`, `export`, `source ~/.zshrc`) as part of maintaining a usable macOS development workstation.

## Development Environment Setup

The knowledge base now includes a focused development environment setup source covering Ruby and iOS toolchain preparation.

### Ruby Environment with rbenv

The standard pattern for Ruby version management on macOS:

1. Install rbenv via Homebrew: `brew install rbenv`
2. Initialize: `rbenv init`
3. Add shell integration to `~/.zshrc`: `eval "$(rbenv init -)"`
4. Install a Ruby version: `rbenv install 3.1.0`
5. Set global default: `rbenv global 3.1.0`

This follows the general [[language-runtime-version-management]] pattern used across multiple language ecosystems.

### CocoaPods for iOS Development

After Ruby is configured via rbenv:

```bash
sudo gem install cocoapods
```

CocoaPods is the traditional dependency manager for iOS/macOS projects, managing third-party libraries through a `Podfile`. Swift Package Manager is the modern Apple-native alternative, but CocoaPods remains widely used in existing projects.

### Development Environment Dependency Chain

The current sources imply a layered dependency chain for iOS development:

```
Homebrew → rbenv → Ruby → gem → CocoaPods
```

Each layer depends on the previous one being correctly configured.

## Installer and Recovery Surface

The earlier macOS source documents a minimal workflow for preparing an installation USB:

1. Use a 16 GB or larger USB drive.
2. Format the drive before media creation.
3. Download the full macOS installer app.
4. Run the version-specific `createinstallmedia` command.

Example:

```bash
sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```

Covered versions in the source:

- Ventura
- Monterey
- Catalina

Installer media alone is not always enough. For a downgrade from macOS 14 to 13, the source also requires:

- entering `Recovery`
- opening `启动安全性实用工具`
- changing the security posture to `无安全性`
- allowing external boot
- then selecting the installer disk at startup

This makes startup policy part of the installation workflow rather than a separate afterthought. The current recovery-oriented knowledge is summarized in [[macos-usb-installer-creation]], [[bootable-os-installer-media]], and [[startup-security-and-external-boot-policy]].
The earlier source also points to `SMC` and `NVRAM` reset procedures as fallback troubleshooting steps when external boot or installer startup behaves abnormally.

## Documentation Notes

- Distinguish portable Unix/BSD commands from Apple-specific interfaces such as `diskutil`, `launchctl`, `pmset`, `scutil`, and `defaults`.
- Distinguish GUI `系统设置` and app-preference guidance from command-line control surfaces; not every useful Mac setting has a good terminal-native representation.
- Distinguish read-only inspection commands from mutating commands such as `rm -rf ~/.Trash/*`, `kill -9`, `shutdown`, route changes, firewall toggles, and `defaults write`.
- When documenting `defaults` examples, explicitly name the affected domain/key and the process that must be restarted for the UI to reflect the change.
- When documenting shortcuts, input sources, or trackpad behavior, call out whether the page is describing a platform default, a recommended configuration, or a user-customized mapping.
- Treat browser workarounds such as disabling Chrome preloading as version-sensitive application guidance, not as timeless macOS truths.
- Keep version-specific installer names exact, because `createinstallmedia` paths depend on the app bundle name.
- Call out hardware/version differences explicitly when authoring future Mac documentation.
- Add backup, restore, and rollback guidance before expanding any of the current notes into a full reinstall or workstation-setup guide.

## Coverage Boundaries

Current macOS coverage is broader than before, but the wiki still does **not yet** cover:

- Apple silicon vs Intel / T2 startup-flow differences
- `launchd` domain model and modern `launchctl` lifecycle semantics
- APFS / Disk Utility partitioning guidance and filesystem repair workflows
- SIP, TCC, Full Disk Access, privacy prompts, and code-signing constraints
- Homebrew formula / cask / tap behavior and version-pinning strategy
- Time Machine restore flows, Migration Assistant, and post-install migration concerns
- keyboard remapping strategy, window management, Mission Control, and accessibility-oriented interaction settings
- structured browser troubleshooting guidance beyond the single Chrome preloading workaround currently captured
- Other language runtime setups (Node.js with nvm, Python with pyenv, Go)
- CocoaPods usage (Podfile, pod commands) beyond installation
- Swift Package Manager as an alternative to CocoaPods
- Xcode project configuration and build settings

## Related Pages

- [[macos-common-commands-reference]]
- [[macos-system-settings]]
- [[macos-command-line-operations]]
- [[macos-usb-installer-creation]]
- [[macos-development-environment-setup]]
- [[language-runtime-version-management]]
- [[bootable-os-installer-media]]
- [[startup-security-and-external-boot-policy]]
- [[glossary]]
- [[overview]]

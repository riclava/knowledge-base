---
title: macOS
type: product
created: 2026-04-16
updated: 2026-04-17
sources: [macOS安装U盘制作.md, macOS常用命令.md]
tags: [macos, apple, desktop-os, unix, bsd, command-line, system-administration, networking, diskutil, launchctl, pmset, homebrew, xcode, defaults, recovery, createinstallmedia, bootable-media, startup-security, downgrade, troubleshooting]
---

# macOS

Apple desktop operating system page covering both installer/recovery workflows and day-to-day command-line administration across system state, disks, networking, services, power, developer tooling, and UI preferences.

---

## Overview

In the current knowledge base, macOS is now represented through two complementary operational lenses:

- reinstall / downgrade preparation through installer media, Recovery, and startup-security changes
- day-to-day local administration through a mix of BSD/Unix commands and Apple-specific control surfaces

This makes macOS in this wiki not just a GUI desktop system, but also a developer workstation and locally administered Unix-like platform.

## Product Snapshot

| Aspect | Details |
|---|---|
| Product type | Apple desktop operating system and local developer workstation platform |
| Command model | Mix of BSD/Unix tools plus Apple-specific interfaces |
| Apple-specific interfaces in current sources | `createinstallmedia`, `system_profiler`, `diskutil`, `launchctl`, `pmset`, `scutil`, `defaults`, `socketfilterfw` |
| Developer-tooling layer | `Homebrew`, Xcode Command Line Tools, shell environment variables |
| Daily administration surfaces | system info, storage, networking, processes, services, power, users, clipboard/screenshot/open commands, Finder/Dock defaults |
| Installer distribution | App Store installer apps, with older versions sometimes discovered through web listings |
| Recovery control surface | `Recovery` -> `Startup Security Utility` |

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
- Distinguish read-only inspection commands from mutating commands such as `rm -rf ~/.Trash/*`, `kill -9`, `shutdown`, route changes, firewall toggles, and `defaults write`.
- When documenting `defaults` examples, explicitly name the affected domain/key and the process that must be restarted for the UI to reflect the change.
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

## Related Pages

- [[macos-common-commands-reference]]
- [[macos-command-line-operations]]
- [[macos-usb-installer-creation]]
- [[bootable-os-installer-media]]
- [[startup-security-and-external-boot-policy]]
- [[glossary]]
- [[overview]]

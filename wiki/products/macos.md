---
title: macOS
type: product
created: 2026-04-16
updated: 2026-04-16
sources: [macOS安装U盘制作.md]
tags: [macos, apple, desktop-os, recovery, createinstallmedia, bootable-media, startup-security, downgrade, troubleshooting]
---

# macOS

Apple desktop operating system page currently focused on bootable installer creation, Recovery-mode startup controls, and downgrade/reinstallation preparation.

---

## Overview

In the current knowledge base, macOS is represented through an operational recovery/install lens rather than day-to-day desktop usage. The source emphasizes that a successful reinstall or downgrade depends on three separate surfaces:

- obtaining a compatible full installer
- creating bootable external media
- ensuring the Mac is allowed to boot from that external media

## Key Characteristics

| Aspect | Details |
|---|---|
| Installer distribution | App Store installer apps, with older versions sometimes discovered through web listings |
| Media creation tool | `createinstallmedia` inside `Install macOS <version>.app` |
| External boot entry | Startup-time `Option (Alt)` boot selection |
| Recovery control surface | `Recovery` -> `Startup Security Utility` |
| Fallback troubleshooting | `NVRAM` and `SMC` reset references |

## Bootable Installer Creation

The current source documents a minimal workflow for preparing an installation USB:

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

See [[macos-usb-installer-creation]] for the step-by-step source summary.

## Recovery and Startup Policy

The source shows that installer media alone is not always enough. For a downgrade from macOS 14 to 13, it requires:

- entering `Recovery`
- opening `启动安全性实用工具`
- changing the security posture to `无安全性`
- allowing external boot
- then selecting the installer disk at startup

This makes startup policy part of the installation workflow, not a separate troubleshooting afterthought.

See [[startup-security-and-external-boot-policy]] for the extracted concept.

## Troubleshooting Surfaces

The current source points to two fallback recovery actions:

- reset `SMC`
- reset `NVRAM`

The source uses these as recovery aids when external-media boot or install startup behavior is abnormal, but it does not document the detailed procedures inline.

## Documentation Notes

- Distinguish **installer acquisition**, **USB creation**, and **boot authorization** as separate steps.
- Keep version-specific installer names exact, because `createinstallmedia` paths depend on the app bundle name.
- Call out hardware/version differences explicitly when authoring future Mac documentation, because the current source does not do so.
- Add backup and restore guidance before expanding this into a full reinstall guide.

## Coverage Boundaries

Current macOS coverage is intentionally narrow. The wiki does **not yet** cover:

- Apple silicon vs Intel startup-flow differences
- APFS / Disk Utility partitioning guidance
- Time Machine restore flows
- post-install migration or activation concerns

## Related Pages

- [[macos-usb-installer-creation]]
- [[bootable-os-installer-media]]
- [[startup-security-and-external-boot-policy]]
- [[glossary]]
- [[overview]]

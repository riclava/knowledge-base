---
title: macOS USB installer creation
type: source
created: 2026-04-16
updated: 2026-04-16
sources: [macOS安装U盘制作.md]
tags: [source, macos, apple, usb-installer, bootable-media, createinstallmedia, recovery, startup-security, downgrade, troubleshooting]
---

# macOS USB Installer Creation

Compact macOS runbook for creating a bootable USB installer, booting from external media, and relaxing startup security for a downgrade workflow.

---

## Source Facts

| Field | Value |
|---|---|
| Original file | `raw/OS/MacOS/macOS安装U盘制作.md` |
| Language | Chinese |
| Scope | macOS installation USB creation and external-boot install flow |
| Media requirement | 16 GB or larger USB drive |
| Covered installers | Ventura, Monterey, Catalina |
| Linked references | Apple support pages for bootable installers, SMC reset, and NVRAM reset |

## Workflow Summary

This source gives a compact operator-oriented workflow for reinstalling or downgrading macOS with an external USB installer:

1. **Check compatibility** — confirm the target macOS version is supported on the target Mac.
2. **Prepare the USB drive** — format it before writing installer media.
3. **Download the full installer app** — usually from the App Store; older versions may require opening the App Store listing from the web first.
4. **Create the installer media** — run `createinstallmedia` from the downloaded installer bundle.
5. **Boot from external media** — hold `Option (Alt)` at power-on and choose the USB drive.
6. **Handle downgrade gating** — for a macOS 14 -> 13 downgrade, enter Recovery and relax startup-security settings so the Mac can boot externally.
7. **Use firmware resets as fallback** — the source points to SMC and NVRAM reset procedures when the Mac does not boot as expected.

## Media Preparation

The source specifies these prerequisites:

- Capacity: 16 GB or larger USB drive
- Format: `MacOS 扩展（日志式）`
- Scheme: `主引导记录`

The source treats formatting as mandatory, but does not explain how to verify the resulting volume name or partition layout before running the installer command.

## Key Commands

### Ventura

```bash
sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```

### Monterey

```bash
sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```

### Catalina

```bash
sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```

## Downgrade Preparation

For downgrading from macOS 14 to 13, the source adds a recovery-side authorization path:

1. Shut down while running macOS 14
2. Enter `Recovery` with `Command + R`
3. Open `实用工具 > 启动安全性实用工具`
4. Set security to `无安全性`
5. Enable `允许外部启动`
6. Reboot and choose the external disk with `Option`

## Documentation Risks and Gaps

| Risk / Gap | Why it matters |
|---|---|
| Installer names are hard-coded per version | The `createinstallmedia` path changes with the exact installer app name |
| Media creation and boot authorization are separate | A correctly prepared USB still may not boot until startup-security settings allow it |
| Hardware differences are not described | The source does not distinguish Intel, T2-equipped, or Apple silicon startup flows |
| Data-protection steps are omitted | The source assumes reinstall/downgrade intent but does not mention backup, erase strategy, or post-install migration |

## Generic Pattern Extracted

| Phase | Core action |
|---|---|
| Compatibility | Confirm the target version is supported on the target machine |
| Media prep | Format removable storage to a known-good state |
| Installer acquisition | Obtain the full installer application, not just an update stub |
| Media creation | Use the platform-provided installer-media tool |
| Boot selection | Choose the external installer from the startup picker |
| Security gating | Adjust recovery/startup policy if external boot is blocked |
| Recovery fallback | Use firmware resets when startup state becomes inconsistent |

## Relationship to Existing Wiki Pages

- [[macos]] — product page that collects the current macOS operational surface in this wiki
- [[bootable-os-installer-media]] — generalized pattern extracted from this runbook
- [[startup-security-and-external-boot-policy]] — concept page for external boot gating and recovery-side authorization
- [[glossary]] — canonical terminology for `macOS`, `createinstallmedia`, `Recovery`, `NVRAM`, and `SMC`

## Related Pages

- [[macos]]
- [[bootable-os-installer-media]]
- [[startup-security-and-external-boot-policy]]
- [[glossary]]
- [[overview]]

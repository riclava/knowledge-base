---
title: Inno Setup
type: product
created: 2026-04-17
updated: 2026-04-17
sources: [Windows开发相关.md]
tags: [inno-setup, windows, installer, packaging, pascal-script, service-installation, multilingual-installer, product]
---

# Inno Setup

Windows installer authoring tool centered on section-based scripts, Pascal-style lifecycle hooks, multilingual packaging, and post-install execution flows.

## Product Snapshot

| Aspect | Details |
|---|---|
| Product type | Windows installer compiler / scripting tool |
| Script structure shown in current source | `#define` macros + section-based script (`[Setup]`, `[Languages]`, `[Files]`, `[Icons]`, `[Run]`, `[UninstallRun]`) |
| Custom code surface | `[Code]` section with Pascal-style `InitializeSetup()` |
| Current payload style | Main executable plus auxiliary installer executable |
| Current post-install behavior | Hidden execution of helper installer and service install/start commands |
| Current uninstall behavior | Hidden execution of app-defined uninstall/stop/uninstall commands |
| Localization surface | English and Simplified Chinese installer languages |

## Core Capability Surface in This Source

### Declarative Installer Structure

- The source uses section blocks to separate product metadata, language resources, files, icons, install-time execution, and uninstall-time execution.
- That makes Inno Setup scripts readable as lifecycle documents, not just as build artifacts.

### Pascal-style Lifecycle Hooking

- `InitializeSetup()` shows that Inno Setup can run custom code before installation proceeds.
- In the current example, the hook launches an external program and waits for it to finish, which establishes a simple preflight gate.

### Service-oriented Post-install Actions

- The source's `[Run]` section is not limited to launching the app UI; it performs service installation and service startup actions.
- `[UninstallRun]` mirrors that with explicit teardown steps, so the installer script becomes part of the service lifecycle contract.

### Build-time Parameterization

- Product metadata is centralized through `#define` macros, and one payload path is read from `GetEnv('MOLAN_RELEASE_DIR')`.
- This indicates a template-friendly authoring style where build output location and installer metadata can vary without rewriting the whole script.

## Why It Matters

- Inno Setup sits at the boundary between build output and user-visible deployment, which makes it a recurring documentation target for Windows-native products.
- The current source shows why installer docs must go beyond "double-click the setup EXE" and explain what the script actually does before and after file copy.
- For the wiki, this page provides a reusable anchor for future Windows packaging notes without forcing them into the broader [[windows]] page.

## Documentation Notes

- Preserve exact section names such as `[Run]` and `[UninstallRun]` when behavior depends on them.
- Call out whether a code hook runs before install, during install, after install, or during uninstall.
- Distinguish installer-owned behavior from application-owned behavior; in the current source, service install/start logic belongs to the packaged executable, not to Inno Setup itself.
- When scripts use hidden execution flags, document the resulting observability tradeoff and where failures would surface.

## Known Gaps from This Source

- No coverage of silent install switches, upgrades, patching, prerequisites, signing, elevation prompts, or rollback behavior.
- No coverage of registry keys, start-menu groups beyond one shortcut, file associations, or per-user vs per-machine installation choices.
- No comparison against MSI, WiX, NSIS, or other Windows packaging approaches.

## Related Pages

- [[windows-development-related]]
- [[windows]]
- [[glossary]]
- [[overview]]

# Wiki Index

Master catalog of all pages. The LLM reads this first when answering queries to find relevant pages. Updated on every ingest.

---

## How to Read This Index

Each entry follows this format:
```
- \[\[filename\]\] — one-line summary | type | last updated
```

---

## Core Files

| Page | Summary | Updated |
|---|---|---|
| [[overview]] | High-level synthesis of the entire knowledge base | 2026-04-15 |
| [[glossary]] | Living terminology, definitions, and style conventions | 2026-04-15 |

---

## Sources

*One entry per raw document ingested. Add entries here after each ingest.*

- [[2025-technical-line-summary]] — Annual review of 2025 technical-line execution and 2026 AI/platform strategy | source | 2026-04-14
- [[bash-syntax-and-scripting-reference]] — Practical Bash reference covering syntax, scripting structure, I/O, and defensive shell patterns | source | 2026-04-15
- [[centos6-archive-repository-workaround]] — Emergency recovery note for restoring CentOS 6 `yum` access by switching from dead mirrors to archive/vault repositories | source | 2026-04-15
- [[centos7-kernel-upgrade-via-elrepo]] — Step-by-step CentOS 7 runbook for installing an ELRepo long-term kernel, selecting the GRUB boot entry, and validating the rebooted host | source | 2026-04-15
- [[centos7-openssl-and-openssh-upgrade-from-source]] — CentOS 7 runbook for rebuilding OpenSSL and OpenSSH from source, cutting over `sshd`, and managing the remote-access risks of manual replacement | source | 2026-04-15
- [[centos7-offline-docker-install-troubleshooting]] — CentOS 7 troubleshooting case showing that an old kernel broke Docker bridge networking despite successful offline installation | source | 2026-04-15
- [[centos7-samba-share-setup]] — Quick CentOS 7 note for exporting a Linux directory through Samba and mapping it as a Windows network drive, with security-hardening caveats | source | 2026-04-15
- [[centos7-system-parameter-tuning]] — CentOS 7 tuning note covering `nofile`, `fs.file-max`, `systemd` `LimitNOFILE`, and TCP listen backlog parameters | source | 2026-04-15
- [[centos7-os-initialization-workflow]] — CentOS 7 initialization runbook covering minimal install, account/network config, SSH port, mirrors, NTP, security policy simplification, and base tools | source | 2026-04-15
- [[linux-common-commands-reference]] — Linux command cheat sheet covering file, text, process, network, permissions, cron, and logs | source | 2026-04-15
- [[moshi-neural-audio-codec-architecture-analysis]] — Architecture analysis of Moshi, Mimi, and Neural Audio Codec as speech LLM infrastructure | source | 2026-04-14
- [[ubuntu2004-docker-dev-environment-setup]] — Minimal recipe for creating an Ubuntu 20.04 development environment using Docker with container keep-alive and APT mirror configuration | source | 2026-04-15
- [[ubuntu2204-openssh-upgrade-from-source]] — Ubuntu 22.04 runbook for upgrading OpenSSH from upstream portable source with in-place binary replacement | source | 2026-04-15
- [[netplan-configuration-guide]] — Comprehensive Ubuntu Netplan reference covering YAML syntax, DHCP/static IP, VLAN, bonding, bridging, and safe testing workflow | source | 2026-04-15
- [[ubuntu-common-issues-and-optimization]] — Ubuntu troubleshooting and optimization reference covering system tuning, APT management, DNS port conflicts, and VMware multipath errors | source | 2026-04-15
- [[ubuntu-kernel-version-switching]] — Ubuntu runbook for installing a specific kernel version via APT, switching the GRUB default boot entry, and validating the rebooted host | source | 2026-04-15
- [[engineering-thinking-framework]] — Engineering-thinking presentation framing software R&D as abstraction, modeling, system decomposition, and validation rather than mere coding | source | 2026-04-15
- [[vim-usage-and-configuration-reference]] — Practical Vim reference covering vimrc defaults, editing commands, and plugin setup | source | 2026-04-15

---

## Features

*One entry per product feature documented.*

*(Empty — will populate as sources are ingested.)*

---

## Products

*One entry per product or tool.*

- [[bash]] — Common Linux shell and scripting runtime for command execution, automation, and task orchestration | product | 2026-04-15
- [[centos]] — RPM/YUM-based enterprise Linux distribution family with repository lifecycle, alternate kernel upgrade paths, and version-specific host-compatibility behavior | product | 2026-04-15
- [[docker]] — Container runtime/tooling page covering host-kernel-dependent networking behavior, validation, kernel-remediation paths, and containerized development environment patterns | product | 2026-04-15
- [[linux]] — Linux operating system and command-line operating surface for files, processes, networking, logging, package-source maintenance, and kernel/boot management | product | 2026-04-15
- [[openssh]] — SSH client/server suite page focused on source-built service replacement across CentOS 7 and Ubuntu 22.04, covering authentication policy and remote-access continuity risks | product | 2026-04-15
- [[openssl]] — TLS/crypto library page focused on source installation, linker-path integration, and its role as a dependency for higher-level services | product | 2026-04-15
- [[samba]] — SMB/CIFS file-sharing server used to expose Linux directories to Windows clients with share-level auth and path masks | product | 2026-04-15
- [[ubuntu]] — Debian-based Linux distribution with APT package management, Netplan network configuration, kernel version management, system optimization, and source-build upgrade paths | product | 2026-04-15
- [[moshi]] — Speech-native LLM focused on full-duplex, low-latency voice interaction | product | 2026-04-14
- [[mimi]] — Neural Audio Codec and tokenizer used to turn speech into low-rate discrete tokens | product | 2026-04-14
- [[vim]] — Modal terminal editor focused on composable text editing and lightweight workflow customization | product | 2026-04-15

---

## Personas

*One entry per user persona or audience segment.*

- [[technical-line-leader]] — Technical management persona responsible for delivery, platform reuse, stability, AI integration, and team engineering-growth enablement | persona | 2026-04-15
- [[growing-engineer]] — Engineer persona moving from feature implementation toward abstraction, system modeling, and validation-led design | persona | 2026-04-15

---

## Concepts

*One entry per core domain concept.*

- [[full-lifecycle-delivery-capability]] — End-to-end software delivery capability grounded in problem framing, design, implementation, release, and operations | concept | 2026-04-15
- [[ai-enabled-software-delivery]] — AI as an integrated delivery capability that still depends on human abstraction, modeling, and validation | concept | 2026-04-15
- [[engineering-mindset]] — Foundational engineering-thinking concept for turning real-world problems into computable systems | concept | 2026-04-15
- [[state-and-data-flow-modeling]] — System-modeling approach that describes behavior through state transitions, data movement, and boundaries | concept | 2026-04-15
- [[validation-driven-design]] — Engineering method for reducing uncertainty before coding through simulation, diagrams, and edge-case analysis | concept | 2026-04-15
- [[container-network-namespace-support]] — Host-kernel support and remediation signals required for Docker bridge networking, veth linkage, and namespace inspection | concept | 2026-04-15
- [[containerized-development-environment]] — Pattern for creating isolated, reproducible development environments using containers with keep-alive processes and scripted initialization | concept | 2026-04-15
- [[file-descriptor-and-tcp-backlog-tuning]] — Layered Linux capacity-tuning concept separating `nofile`, kernel file-table limits, `systemd` unit limits, and TCP backlog parameters | concept | 2026-04-15
- [[platform-foundation]] — Shared platform and governance base used to increase reuse and reduce siloed systems | concept | 2026-04-14
- [[observability-and-reliability]] — Observability, incident handling, and reliability as non-negotiable engineering fundamentals | concept | 2026-04-14
- [[speech-native-llm]] — Architecture pattern that models and generates speech directly instead of centering text | concept | 2026-04-14
- [[neural-audio-codec]] — Audio tokenizer infrastructure layer that discretizes speech for LLM use | concept | 2026-04-14
- [[full-duplex-speech-interaction]] — Real-time interaction model that supports simultaneous listening, speaking, and interruption | concept | 2026-04-14
- [[kernel-upgrade-and-boot-management]] — Practice of upgrading or switching Linux kernel versions, choosing a boot target, and validating compatibility outcomes before cleanup, covering both CentOS 7 and Ubuntu patterns | concept | 2026-04-15
- [[legacy-repository-repointing]] — Practice of redirecting package managers from retired mirrors to static archive/vault repositories for legacy systems | concept | 2026-04-15
- [[modal-editing]] — Editing model where key behavior changes by mode and commands compose across motions and text objects | concept | 2026-04-15
- [[shell-scripting]] — Automation practice that composes shell builtins, commands, and Unix I/O primitives into reusable workflows | concept | 2026-04-15
- [[source-built-package-replacement]] — Practice of compiling upstream software outside the package manager, then manually wiring it into system paths, services, and rollback strategy, covering both CentOS 7 and Ubuntu 22.04 patterns | concept | 2026-04-15
- [[linux-command-line-operations]] — Practice of inspecting and administering Linux systems through composable command-line tools, including package-source repair, kernel activation, and service validation | concept | 2026-04-15
- [[smb-file-sharing]] — Practice of exposing directories over SMB so Windows clients can map Linux-hosted network drives with explicit auth and permission boundaries | concept | 2026-04-15
- [[os-initialization-workflow]] — Structured workflow for configuring a fresh Linux install from bare metal to usable baseline, covering install options, accounts, network, remote access, package sources, time sync, security policy, and base tools | concept | 2026-04-15
- [[network-configuration]] — Practice of defining network interface settings, IP addressing, routing, and DNS through declarative configuration files, enabling reproducible network setup | concept | 2026-04-15
- [[systemd-resolved-dns-management]] — Practice of configuring systemd-resolved DNS service, managing port 53 conflicts, and controlling DNS stub listener behavior on modern Ubuntu | concept | 2026-04-15

---

## Style Rules

*One entry per writing convention or style guideline.*

*(Empty — will populate as sources are ingested.)*

---

## Analyses

*One entry per synthesized output: comparison tables, gap analyses, outlines.*

*(Empty — file your first query answer here to start compounding.)*

---

## Index Maintenance Notes

- Add new pages immediately after creation
- Update the "last updated" date when a page changes substantially
- Mark orphan pages with `⚠️ orphan` until they gain inbound links
- If a category grows beyond 10 pages, consider adding sub-sections

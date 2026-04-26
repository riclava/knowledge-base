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
| [[overview]] | High-level synthesis of the entire knowledge base | 2026-04-26 |
| [[glossary]] | Living terminology, definitions, and style conventions | 2026-04-26 |

---

## Sources

*One entry per raw document ingested. Add entries here after each ingest.*

- [[2025-technical-line-summary]] — Annual review of 2025 technical-line execution and 2026 AI/platform strategy | source | 2026-04-14
- [[cto-responsibilities]] — Role note defining CTO mission, evaluation signals, time allocation, and cross-functional responsibilities | source | 2026-04-19
- [[ai-full-stack-development-onboarding]] — AI-era onboarding deck covering environment setup, AI collaboration, MCP/Steering, DevOps flow, and a starter full-stack stack | source | 2026-04-15
- [[bash-syntax-and-scripting-reference]] — Practical Bash reference covering syntax, scripting structure, I/O, and defensive shell patterns | source | 2026-04-15
- [[centos6-archive-repository-workaround]] — Emergency recovery note for restoring CentOS 6 `yum` access by switching from dead mirrors to archive/vault repositories | source | 2026-04-15
- [[centos7-kernel-upgrade-via-elrepo]] — Step-by-step CentOS 7 runbook for installing an ELRepo long-term kernel, selecting the GRUB boot entry, and validating the rebooted host | source | 2026-04-15
- [[centos7-openssl-and-openssh-upgrade-from-source]] — CentOS 7 runbook for rebuilding OpenSSL and OpenSSH from source, cutting over `sshd`, and managing the remote-access risks of manual replacement | source | 2026-04-15
- [[centos7-offline-docker-install-troubleshooting]] — CentOS 7 troubleshooting case showing that an old kernel broke Docker bridge networking despite successful offline installation | source | 2026-04-15
- [[centos7-samba-share-setup]] — Quick CentOS 7 note for exporting a Linux directory through Samba and mapping it as a Windows network drive, with security-hardening caveats | source | 2026-04-15
- [[centos7-system-parameter-tuning]] — CentOS 7 tuning note covering `nofile`, `fs.file-max`, `systemd` `LimitNOFILE`, and TCP listen backlog parameters | source | 2026-04-15
- [[centos7-os-initialization-workflow]] — CentOS 7 initialization runbook covering minimal install, account/network config, SSH port, mirrors, NTP, security policy simplification, and base tools | source | 2026-04-15
- [[linux-common-commands-reference]] — Linux command cheat sheet covering file, text, process, network, permissions, cron, and logs | source | 2026-04-15
- [[macos-common-commands-reference]] — macOS command cheat sheet covering system info, storage, network diagnostics, processes, services, power, developer tooling, and desktop automation | source | 2026-04-17
- [[macos-development-environment-setup]] — Compact macOS development environment setup note covering Ruby version management with rbenv and CocoaPods installation | source | 2026-04-17
- [[macos-system-settings]] — Compact macOS workstation-setup note covering shortcut preferences, English-first input configuration, trackpad behavior, and a Chrome typing-lag workaround | source | 2026-04-17
- [[macos-usb-installer-creation]] — Compact macOS runbook for creating a bootable USB installer, booting from external media, and handling downgrade-time startup-security changes | source | 2026-04-16
- [[moshi-neural-audio-codec-architecture-analysis]] — Architecture analysis of Moshi, Mimi, and Neural Audio Codec as speech LLM infrastructure | source | 2026-04-14
- [[ubuntu2004-docker-dev-environment-setup]] — Minimal recipe for creating an Ubuntu 20.04 development environment using Docker with container keep-alive and APT mirror configuration | source | 2026-04-15
- [[ubuntu2204-openssh-upgrade-from-source]] — Ubuntu 22.04 runbook for upgrading OpenSSH from upstream portable source with in-place binary replacement | source | 2026-04-15
- [[netplan-configuration-guide]] — Comprehensive Ubuntu Netplan reference covering YAML syntax, DHCP/static IP, VLAN, bonding, bridging, and safe testing workflow | source | 2026-04-15
- [[ubuntu-common-issues-and-optimization]] — Ubuntu troubleshooting and optimization reference covering system tuning, APT management, DNS port conflicts, and VMware multipath errors | source | 2026-04-15
- [[ubuntu-kernel-version-switching]] — Ubuntu runbook for installing a specific kernel version via APT, switching the GRUB default boot entry, and validating the rebooted host | source | 2026-04-15
- [[engineering-thinking-framework]] — Engineering-thinking presentation framing software R&D as abstraction, modeling, system decomposition, and validation rather than mere coding | source | 2026-04-15
- [[grammar]] — English grammar learning map for Chinese learners covering sentence structure, word classes, verb systems, clauses, writing conventions, common errors, and practice workflows | source | 2026-04-26
- [[distributed-systems-core-principles]] — Structured distributed-systems study note covering uncertainty intuition, CAP/FLP/PACELC, consistency, consensus, replication/sharding, logical clocks, and resilience patterns | source | 2026-04-17
- [[linux-foundations-and-testing-special-topic]] — Training deck that teaches Linux as an engineering mental model and pairs it with layered software-testing architecture | source | 2026-04-16
- [[vim-usage-and-configuration-reference]] — Practical Vim reference covering vimrc defaults, editing commands, and plugin setup | source | 2026-04-15
- [[windows-development-related]] — Compact Windows-native development note covering Inno Setup packaging, service bootstrap sequencing, and WMI-based machine fingerprint generation | source | 2026-04-17
- [[windows-system-settings]] — Compact Windows workstation-administration note covering Windows 10 auto-update suppression and PowerShell firewall rules for debug ports | source | 2026-04-17

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
- [[inno-setup]] — Windows installer authoring tool centered on section-based scripts, Pascal-style hooks, multilingual packaging, and service-oriented post-install actions | product | 2026-04-17
- [[linux]] — Linux operating system and engineering mental model spanning command-line operations, permissions, observability, and system design principles | product | 2026-04-16
- [[macos]] — Apple desktop operating system page covering installer/recovery workflows, interactive desktop preferences, command-line administration, and development environment setup across input, trackpad, disks, networking, services, power, and developer tooling | product | 2026-04-17
- [[openssh]] — SSH client/server suite page focused on source-built service replacement across CentOS 7 and Ubuntu 22.04, covering authentication policy and remote-access continuity risks | product | 2026-04-15
- [[openssl]] — TLS/crypto library page focused on source installation, linker-path integration, and its role as a dependency for higher-level services | product | 2026-04-15
- [[samba]] — SMB/CIFS file-sharing server used to expose Linux directories to Windows clients with share-level auth and path masks | product | 2026-04-15
- [[ubuntu]] — Debian-based Linux distribution with APT package management, Netplan network configuration, kernel version management, system optimization, and source-build upgrade paths | product | 2026-04-15
- [[windows]] — Microsoft Windows operating system page covering native packaging, workstation administration, update/firewall control surfaces, WMI metadata access, and SMB client behavior | product | 2026-04-17
- [[moshi]] — Speech-native LLM focused on full-duplex, low-latency voice interaction | product | 2026-04-14
- [[mimi]] — Neural Audio Codec and tokenizer used to turn speech into low-rate discrete tokens | product | 2026-04-14
- [[vim]] — Modal terminal editor focused on composable text editing and lightweight workflow customization | product | 2026-04-15

---

## Personas

*One entry per user persona or audience segment.*

- [[ai-era-full-stack-beginner]] — Beginner persona learning AI-assisted environment setup, Git/CI/CD flow, and minimum end-to-end full-stack delivery | persona | 2026-04-15
- [[cto]] — Executive technical leader responsible for long-term technology strategy, external technical narrative, and cross-functional alignment | persona | 2026-04-19
- [[technical-line-leader]] — Technical management persona focused on internal delivery, platform reuse, stability, AI integration, and team engineering-growth enablement | persona | 2026-04-19
- [[growing-engineer]] — Engineer persona moving from feature implementation toward abstraction, system modeling, and validation-led design | persona | 2026-04-15
- [[chinese-english-grammar-learner]] — Chinese-speaking English learner persona focused on sentence skeletons, verb systems, clause boundaries, and writing self-checks | persona | 2026-04-26

---

## Concepts

*One entry per core domain concept.*

- [[full-lifecycle-delivery-capability]] — End-to-end software delivery capability grounded in problem framing, layered quality gates, release, and operations | concept | 2026-04-16
- [[technology-strategy-and-business-alignment]] — Executive alignment practice that keeps technology strategy, roadmap, and outward technical narrative tied to business goals | concept | 2026-04-19
- [[ai-enabled-software-delivery]] — AI as an integrated delivery capability that still depends on human abstraction, modeling, validation, and workflow guardrails | concept | 2026-04-15
- [[bootable-os-installer-media]] — Practice of writing a full OS installer to external media so installation, reinstall, downgrade, and recovery can happen outside the running system | concept | 2026-04-16
- [[model-context-protocol]] — Protocol layer that lets AI systems call external tools and data sources as part of execution workflows | concept | 2026-04-15
- [[project-steering-context]] — Persistent project context that teaches AI the stack, rules, and domain constraints of a repository | concept | 2026-04-15
- [[devops-delivery-pipeline]] — Delivery-loop concept connecting local development, layered test gates, deployment, and runtime feedback | concept | 2026-04-16
- [[software-testing-architecture]] — Layered testing model combining unit/property tests, integration tests, E2E, regression data, and CI staging | concept | 2026-04-16
- [[property-based-testing]] — Testing style that validates invariants over generated input spaces instead of only checking hand-picked examples | concept | 2026-04-16
- [[engineering-mindset]] — Foundational engineering-thinking concept for turning real-world problems into computable systems, including distributed trade-off reasoning under uncertainty | concept | 2026-04-17
- [[state-and-data-flow-modeling]] — System-modeling approach that describes behavior through state transitions, data movement, and boundaries, now linked to distributed ordering and replication concerns | concept | 2026-04-17
- [[distributed-systems-foundations]] — Boundary concept for network uncertainty, failure models, CAP/FLP/PACELC, and why distributed design is fundamentally trade-off-driven | concept | 2026-04-17
- [[consistency-models]] — Read-visibility spectrum from linearizability through causal and eventual consistency, chosen by business tolerance for stale or out-of-order views | concept | 2026-04-17
- [[distributed-consensus]] — Multi-node agreement concept centered on Paxos/Raft, majority quorums, leader election, and replicated-log safety | concept | 2026-04-17
- [[data-replication-and-partitioning]] — Data-layout concept covering replica strategies, sharding choices, consistent hashing, conflict resolution, and expansion/migration trade-offs | concept | 2026-04-17
- [[logical-time-and-causality]] — Ordering concept that replaces unreliable physical clocks with happened-before reasoning, Lamport clocks, vector clocks, and HLC | concept | 2026-04-17
- [[distributed-systems-resilience-patterns]] — Engineering mechanisms such as timeouts, retries, idempotency, quorum, caching, circuit breaking, degradation, and failover | concept | 2026-04-17
- [[validation-driven-design]] — Engineering method for reducing uncertainty before coding through simulation, diagrams, and edge-case analysis | concept | 2026-04-15
- [[unix-philosophy-and-pipeline-thinking]] — Unix-style design model centered on small tools, text interfaces, unified abstractions, and pipeline composition | concept | 2026-04-16
- [[container-network-namespace-support]] — Host-kernel support and remediation signals required for Docker bridge networking, veth linkage, and namespace inspection | concept | 2026-04-15
- [[containerized-development-environment]] — Pattern for creating isolated, reproducible development environments using containers with keep-alive processes and scripted initialization | concept | 2026-04-15
- [[file-descriptor-and-tcp-backlog-tuning]] — Layered Linux capacity-tuning concept separating `nofile`, kernel file-table limits, `systemd` unit limits, and TCP backlog parameters | concept | 2026-04-15
- [[platform-foundation]] — Shared platform and governance base used to increase reuse and reduce siloed systems | concept | 2026-04-14
- [[observability-and-reliability]] — Observability, incident handling, and reliability as engineering fundamentals spanning Linux host signals, distributed-failure handling, monitoring systems, and delivery feedback | concept | 2026-04-17
- [[use-methodology]] — Resource-triage method that inspects utilization, saturation, and errors across CPU, memory, disk, and network | concept | 2026-04-16
- [[speech-native-llm]] — Architecture pattern that models and generates speech directly instead of centering text | concept | 2026-04-14
- [[neural-audio-codec]] — Audio tokenizer infrastructure layer that discretizes speech for LLM use | concept | 2026-04-14
- [[full-duplex-speech-interaction]] — Real-time interaction model that supports simultaneous listening, speaking, and interruption | concept | 2026-04-14
- [[hardware-derived-machine-identifier]] — Practice of collecting multiple hardware properties and hashing them into a derived device fingerprint, called `机器码` in the current Windows source | concept | 2026-04-17
- [[english-grammar-learning-framework]] — Layered English grammar learning model organized around word classes, sentence functions, structural relations, and meaning choices | concept | 2026-04-26
- [[english-sentence-structure]] — Sentence-structure concept centered on finite predicates, five basic sentence patterns, constituents, phrases, clauses, and punctuation boundaries | concept | 2026-04-26
- [[english-verb-system]] — Verb-system concept covering finite and non-finite verbs, tense/aspect, voice, mood, modality, and verb-pattern choices | concept | 2026-04-26
- [[english-clause-system]] — Clause-system concept covering noun clauses, relative clauses, adverbial clauses, relation words, and clause-boundary diagnostics | concept | 2026-04-26
- [[english-usage-error-patterns-for-chinese-learners]] — Reusable error-pattern page for Chinese learners covering articles, countability, tense transfer, conjunction duplication, prepositions, clause order, and sentence boundaries | concept | 2026-04-26
- [[kernel-upgrade-and-boot-management]] — Practice of upgrading or switching Linux kernel versions, choosing a boot target, and validating compatibility outcomes before cleanup, covering both CentOS 7 and Ubuntu patterns | concept | 2026-04-15
- [[language-runtime-version-management]] — Practice of using dedicated version managers to install, switch between, and isolate multiple versions of a language runtime on a single development machine | concept | 2026-04-17
- [[legacy-repository-repointing]] — Practice of redirecting package managers from retired mirrors to static archive/vault repositories for legacy systems | concept | 2026-04-15
- [[modal-editing]] — Editing model where key behavior changes by mode and commands compose across motions and text objects | concept | 2026-04-15
- [[shell-scripting]] — Automation practice that composes shell builtins, commands, and Unix I/O primitives into reusable, idempotence-aware workflows | concept | 2026-04-16
- [[startup-security-and-external-boot-policy]] — Control surface that decides whether a device may boot from external installer media and which recovery-time security changes are required | concept | 2026-04-16
- [[source-built-package-replacement]] — Practice of compiling upstream software outside the package manager, then manually wiring it into system paths, services, and rollback strategy, covering both CentOS 7 and Ubuntu 22.04 patterns | concept | 2026-04-15
- [[linux-command-line-operations]] — Practice of inspecting and administering Linux systems through composable command-line tools, including resource observability and service validation | concept | 2026-04-16
- [[macos-command-line-operations]] — Practice of administering macOS through BSD/Unix commands plus Apple-specific interfaces while recognizing GUI system settings and app preferences as adjacent control surfaces | concept | 2026-04-17
- [[smb-file-sharing]] — Practice of exposing directories over SMB so Windows clients can map Linux-hosted network drives with explicit auth and permission boundaries | concept | 2026-04-15
- [[os-initialization-workflow]] — Structured workflow for configuring a fresh Linux install from bare metal to usable baseline, covering install options, accounts, network, remote access, package sources, time sync, security policy, and base tools | concept | 2026-04-15
- [[network-configuration]] — Practice of defining network interface settings, IP addressing, routing, and DNS through declarative configuration files, enabling reproducible network setup | concept | 2026-04-15
- [[systemd-resolved-dns-management]] — Practice of configuring systemd-resolved DNS service, managing port 53 conflicts, and controlling DNS stub listener behavior on modern Ubuntu | concept | 2026-04-15
- [[windows-management-instrumentation]] — Windows system metadata/query surface used in current sources to retrieve BIOS, CPU, motherboard, and disk properties through COM or `wmic` | concept | 2026-04-17

---

## Style Rules

*One entry per writing convention or style guideline.*

- [[english-writing-clarity-and-style]] — English writing style rule that treats grammar accuracy, punctuation boundaries, information flow, and formal clarity as one self-check chain | style | 2026-04-26

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

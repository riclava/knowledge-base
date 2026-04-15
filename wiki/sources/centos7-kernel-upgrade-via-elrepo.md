---
title: CentOS7升级内核
type: source
created: 2026-04-15
updated: 2026-04-15
sources: [CentOS7升级内核.md]
tags: [centos, linux, kernel, elrepo, grub, bootloader, repository, package-management, upgrade, source]
---

这是一份通过 ELRepo 在 CentOS 7 上安装长期维护内核、切换默认 GRUB 启动项并验证升级结果的操作备忘，同时提示了镜像替换、旧内核保留与工具包对齐等维护细节。

## Source Metadata

- Raw source: `raw/OS/Linux/CentOS/CentOS7升级内核.md`
- 时间标记：未注明
- 文档性质：升级操作笔记 / 维护备忘
- 主要对象：CentOS 7、ELRepo、`kernel-lt`、`kernel-ml`、`/etc/default/grub`、`/boot/grub2/grub.cfg`
- 示例目标内核：`kernel-lt-5.4.108-1.el7.elrepo.x86_64`
- 适用场景：CentOS 7 主机需要获得较新的内核能力，或需要通过显式切换默认启动项来完成内核升级时

## What the Source Covers

| 阶段 | 操作 | 观察到的意图 |
|---|---|---|
| 版本基线检查 | `cat /etc/redhat-release`、`uname -r` | 先确认当前发行版和正在运行的内核版本 |
| 检查第三方仓库可用性 | `yum --disablerepo="*" --enablerepo="elrepo-kernel" list available` | 在安装前确认 ELRepo kernel 仓库是否已可用 |
| 建立 ELRepo 信任链 | `yum -y update`、`rpm --import ...RPM-GPG-KEY-elrepo.org`、安装 `elrepo-release` RPM | 让系统具备访问第三方内核仓库的能力 |
| 可选镜像替换 | 将 ELRepo 源改为清华镜像 `baseurl` | 提升下载可达性或速度，但保持仓库结构不变 |
| 选择内核分支 | 再次列出可用包，并注明 `lt` 为长期维护、`ml` 为主线稳定 | 把“升级内核”具体化为选择哪条包分支 |
| 安装内核 | `yum --enablerepo=elrepo-kernel install -y kernel-lt` | 把新内核包装到系统中，但尚未激活为运行内核 |
| 查看启动菜单 | `awk -F\\' '$1=="menuentry " {print i++ " : " $2}' /boot/grub2/grub.cfg` | 显式检查有哪些可启动内核条目以及顺序 |
| 设定默认启动项 | `grub2-set-default 0` 或 `grub-set-default 'CentOS Linux (...)'` | 指定下一次重启默认进入哪一个内核 |
| 固化 GRUB 配置 | 编辑 `/etc/default/grub`，再执行 `grub2-mkconfig -o /boot/grub2/grub.cfg` | 让默认项与实际 GRUB 配置保持一致 |
| 重启与验证 | `reboot` 后执行 `uname -r` | 确认主机已经真的运行在新内核上 |
| 升级后整理 | `rpm -qa | grep kernel`、可选删除旧内核、安装 `yum-utils` / `kernel-lt-tools` | 管理保留版本、工具包和后续维护成本 |

## Core Mental Model Implied by the Source

1. 在 CentOS 7 上“升级内核”不是单条 `yum install` 命令，而是“仓库信任 + 包安装 + 启动项切换 + 重启验证”的组合流程。
2. 新内核装进系统后，并不会自动改变当前运行内核；真正生效还要经过 GRUB 默认项选择和一次成功重启。
3. ELRepo 只是提供另一条内核供应链，是否使用它是一个运维决策，而不是所有问题的默认答案。
4. 升级过程中保留旧内核是一种回滚能力，不应在未验证新内核前过早清理。
5. 这份来源记录的是 ELRepo `kernel-lt` 路线；它补足了“如何升级”的操作细节，但不意味着所有场景都必须放弃 CentOS 官方维护的 3.10 分支。

## Notable Operational Patterns

### Scope Repository Access Precisely

- 来源反复使用 `--disablerepo="*"` 与 `--enablerepo="elrepo-kernel"`，说明它希望把影响范围收敛到目标仓库，而不是让所有源同时参与解析。
- 这种写法对内核升级尤其重要，因为它能减少“从错误仓库拿到包”的不确定性。

### Separate Installation from Activation

- `kernel-lt` 的安装步骤只解决“包是否存在于磁盘上”。
- `grub2-set-default`、修改 `/etc/default/grub` 和 `grub2-mkconfig` 才是在处理“下次启动是否真的会进入新内核”。

### Treat Mirror Replacement as an Environment Optimization

- 清华镜像替换被明确标注为“可选”，说明它属于可达性优化，而不是升级流程不可缺少的逻辑步骤。
- 正式文档应把这种镜像替换写成环境分支，而不是默认主线。

### Verify Boot Choices from Rendered Entries

- 来源不是凭猜测写 `grub2-set-default`，而是先从 `/boot/grub2/grub.cfg` 中枚举 `menuentry`。
- 这体现出一个重要习惯：真正决定启动行为的是最终渲染出来的条目，而不仅是某个包名。

### Delay Kernel Cleanup Until After a Good Reboot

- 来源把删除旧内核放在重启验证之后，并明确标为“可选”。
- 这符合更安全的升级顺序：先保留回滚路径，再考虑减少冗余版本。

## Caveats and Verification Risks

- `yum -y update` 是一次广义系统更新，不只是“为 ELRepo 做准备”；如果整理为正式 runbook，应标出其影响范围和变更窗口要求。
- `grub2-set-default 0` 与把 `/etc/default/grub` 中 `GRUB_DEFAULT=saved` 改为 `0` 属于两套默认项管理方式；来源把它们放在一起但没有解释适用条件，容易让读者混淆。
- 示例中的 `grub-set-default 'CentOS Linux (5.4.96-1.el7.elrepo.x86_64) 7 (Core)'` 与后面展示的 `kernel-lt-5.4.108-1.el7.elrepo.x86_64` 不一致，说明标题式选择必须以目标主机当前实际 `menuentry` 为准。
- `yum remove -y kernel-devel-3.10.0 kernel-3.10.0 kernel-headers-3.10.0` 这类清理步骤只有在确认新内核可稳定启动后才安全；否则会削弱回滚能力。
- 示例里旧的 `kernel-tools*` 与新的 `kernel-lt-tools` 并存，说明“内核升级完成”不等于所有内核相关工具包都已自动对齐。
- 来源没有给出失败回滚、串口/控制台救援、或 `grubby --default-kernel` 之类的补充验证，因此仍更像现场备忘而不是完整生产变更手册。

## Documentation Implications

- 应把它写成“CentOS 7 通过 ELRepo 升级内核并切换默认启动项”的 runbook，而不是笼统的“升级内核命令列表”。
- 应明确区分“更新到 CentOS 官方维护的较新 3.10 内核”和“转向 ELRepo `kernel-lt` / `kernel-ml` 分支”这两类升级路径。
- 应增加升级前检查、重启后验证、失败时回退到旧内核的步骤。
- 如果和 Docker 排障案例一起引用，应说明 `[[centos7-offline-docker-install-troubleshooting]]` 展示的是“宿主机内核基线不足”这一根因，而本页补充的是其中一种明确可执行的升级路径。

## Related Pages

- [[centos]]
- [[linux]]
- [[docker]]
- [[kernel-upgrade-and-boot-management]]
- [[centos7-offline-docker-install-troubleshooting]]
- [[container-network-namespace-support]]
- [[linux-command-line-operations]]
- [[glossary]]
- [[overview]]

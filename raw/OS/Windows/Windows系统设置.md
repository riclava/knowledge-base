# Windows 系统设置

## 关闭 Win10 自动更新

### 1. 禁用 Windows Update 服务

1. 打开任务管理器（`Ctrl + Shift + ESC`）
2. 点击「服务」→「打开服务」
3. 找到 **Windows Update**，右键 → 属性
4. 启动类型设为「**禁用**」
5. 切换到「恢复」选项卡，将三次失败操作都改为「**无操作**」
6. 点击「应用」→「确定」

### 2. 关闭计划任务

1. 右键「此电脑」→「管理」
2. 依次展开：系统工具 → 任务计划程序 → 任务计划程序库 → Microsoft → Windows → Windows Update
3. 将里面的所有选项右键禁用

### 3. 删除升级助手

删除以下文件夹（如存在）：

- `C:\Windows10Upgrade`
- `C:\Windows\UpdateAssistant`
- `C:\Windows\UpdateAssistantV2`

---

## 防火墙操作

### 允许端口监听（用于调试）

```powershell
New-NetFirewallRule -DisplayName "Allow Port 12345" -Direction Inbound -Protocol TCP -LocalPort 12345 -Action Allow
```

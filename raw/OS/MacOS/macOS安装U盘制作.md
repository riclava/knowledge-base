# macOS 安装 U 盘制作

## 准备工作

- 16G 及以上容量的 U 盘
- 系统支持情况查询：<https://eshop.macsales.com/guides/Mac_OS_X_Compatibility>

## 格式化 U 盘

格式选择：**MacOS 扩展（日志式）**，方案选择：**主引导记录**

## 下载系统

从 App Store 下载，例如 Ventura。老版本系统需要通过 Google 搜索，然后通过 Web 页面打开 App Store 才能找到。

## 制作安装盘

打开 Terminal，根据系统版本执行对应命令：

```bash
# Ventura
sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume

# Monterey
sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume

# Catalina
sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```

## 安装系统

按住 **Option (Alt)** 键后按开机键，选择 U 盘启动

## 降级须知

从 macOS 14 降级到 13 的步骤：

1. 在 macOS 14 系统下关机
2. 按住 **Command + R** 进入 Recovery 模式
3. 选择 **实用工具 > 启动安全性实用工具**
4. 设置为 **无安全性**、**允许外部启动**
5. 重启时按 **Option** 键选择外部磁盘启动

## 故障排除

- [重置 SMC](https://support.apple.com/zh-cn/HT201295)
- [重置 NVRAM](https://support.apple.com/zh-cn/HT204063)

## 参考资料

- <https://support.apple.com/zh-cn/HT201372>
- <https://support.apple.com/zh-cn/HT201255#guidelines>

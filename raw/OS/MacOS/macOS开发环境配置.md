# macOS 开发环境配置

## Ruby 环境

使用 rbenv 管理 Ruby 版本：

```bash
# 安装 rbenv
brew install rbenv

# 初始化
rbenv init

# 添加到 ~/.zshrc
eval "$(rbenv init -)"

# 安装指定版本
rbenv install 3.1.0  # 或更新版本，如 3.2.0

# 设置全局版本
rbenv global 3.1.0
```

## CocoaPods

### 前置条件

确保已通过 rbenv 安装了较新版本的 Ruby（参考上方 Ruby 环境配置）

### 安装

```bash
sudo gem install cocoapods
```

---
title: macOS Development Environment Setup
type: source
created: 2026-04-17
updated: 2026-04-17
sources: [macOS开发环境配置.md]
tags: [macos, development-environment, ruby, rbenv, cocoapods, ios, homebrew, version-management, gem]
---

# macOS Development Environment Setup

Compact macOS development environment setup note covering Ruby version management with rbenv and CocoaPods installation for iOS/macOS development.

---

## Source Facts

| Attribute | Value |
|---|---|
| Filename | `macOS开发环境配置.md` |
| Location | `raw/OS/MacOS/` |
| Primary focus | Ruby environment setup and CocoaPods installation |
| Target platform | macOS |
| Package manager used | Homebrew (for rbenv), gem (for CocoaPods) |
| Ruby version manager | rbenv |
| Example Ruby versions | 3.1.0, 3.2.0 |

---

## Content Summary

### Ruby Environment with rbenv

The source documents a standard rbenv-based Ruby version management workflow:

1. **Install rbenv** via Homebrew:
   ```bash
   brew install rbenv
   ```

2. **Initialize rbenv**:
   ```bash
   rbenv init
   ```

3. **Add shell integration** to `~/.zshrc`:
   ```bash
   eval "$(rbenv init -)"
   ```

4. **Install a specific Ruby version**:
   ```bash
   rbenv install 3.1.0  # or 3.2.0
   ```

5. **Set the global default**:
   ```bash
   rbenv global 3.1.0
   ```

### CocoaPods Installation

The source documents CocoaPods installation as a follow-on step after Ruby setup:

- **Prerequisite**: A modern Ruby version installed via rbenv
- **Installation command**:
  ```bash
  sudo gem install cocoapods
  ```

---

## Key Patterns

### Version Manager Pattern

The source follows a common language-runtime version management pattern:

1. Install the version manager (rbenv) via system package manager (Homebrew)
2. Initialize the version manager
3. Add shell integration to the user's shell config
4. Install the desired language version
5. Set the default version

This pattern is reusable across other language ecosystems (Node.js with nvm/fnm, Python with pyenv, etc.).

### Dependency Chain

The source implies a dependency chain for iOS/macOS development:

```
Homebrew → rbenv → Ruby → gem → CocoaPods
```

Each layer depends on the previous one being correctly configured.

---

## Documentation Notes

- The source uses `sudo gem install cocoapods`, which installs CocoaPods system-wide. An alternative is `gem install cocoapods --user-install` for user-local installation, though this requires PATH configuration.
- The source assumes `zsh` as the default shell (standard on modern macOS). For `bash` users, the shell integration line would go in `~/.bash_profile` or `~/.bashrc`.
- The source does not cover rbenv plugin ecosystem (e.g., `ruby-build` for compiling Ruby versions), though `brew install rbenv` typically includes `ruby-build` as a dependency.
- The source does not cover CocoaPods usage (`pod init`, `pod install`, `Podfile` syntax), only installation.

---

## Coverage Boundaries

This source covers:
- rbenv installation and basic configuration
- Ruby version installation and global default setting
- CocoaPods installation

This source does **not** cover:
- Other Ruby version managers (rvm, asdf)
- rbenv plugins or advanced configuration
- CocoaPods usage (Podfile, pod commands)
- Other iOS development tools (Xcode, Swift Package Manager, Fastlane)
- Other language runtime setups (Node.js, Python, Go)

---

## Related Pages

- [[macos]] — macOS product page with development environment section
- [[macos-common-commands-reference]] — broader macOS command reference including Homebrew
- [[macos-command-line-operations]] — macOS command-line operations concept
- [[language-runtime-version-management]] — concept page for version manager patterns
- [[glossary]] — terminology including rbenv, CocoaPods, gem


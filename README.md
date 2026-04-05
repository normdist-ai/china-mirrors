# 🇨🇳 China Mirrors - AI IDE Agent Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/normdist-ai/china-mirrors.svg)](https://github.com/normdist-ai/china-mirrors/stargazers)

[中文版文档](README.zh.md)

Automatically configure domestic mirror sources for various package managers in China, solving dependency download speed issues for developers.

## 🚀 Features

- **11 Package Managers Supported**: pip, npm, yarn, pnpm, cargo, go mod, NuGet, RubyGems, Conda, Gradle, Homebrew
- **PDCA Testing**: Built-in script to test mirror availability and performance
- **Cross-Platform**: Windows, Linux, macOS
- **One-Click Config**: Automated scripts for global and project-level configuration

## 📦 Installation

### For AI IDEs (OpenCode, Cursor, etc.)

Copy the `skills/china-mirrors` folder to your IDE's skills directory:

- **OpenCode**: `~/.config/opencode/skills/china-mirrors`
- **Cursor**: `.cursor/skills/china-mirrors`
- **Lingma**: `.lingma/skills/china-mirrors`

### Manual Usage

```bash
cd skills/china-mirrors
python scripts/config_all.py --all
```

## 📊 Mirror Availability Test

Run the PDCA test script to verify mirror status:

```bash
python scripts/test_mirrors.py --verbose
```

Latest results (2026-04-06): **33 mirrors tested, 24 available**.

## 📁 Project Structure

```
skills/
  china-mirrors/
    SKILL.md              # Skill definition & detailed guide
    README.md             # User documentation
    QUICK_REFERENCE.md    # Quick reference card
    examples.md           # Usage examples
    scripts/
      config_all.py       # One-click config script
      config_pip.py       # Pip specific config
      config_npm.js       # Npm specific config
      test_mirrors.py     # PDCA mirror availability test
    evals/
      evals.json          # Evaluation test cases
```

## 📖 Documentation

See [skills/china-mirrors/README.md](skills/china-mirrors/README.md) for full documentation.

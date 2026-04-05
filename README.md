# 🇨🇳 China Mirrors - AI IDE Agent Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/normdist-ai/china-mirrors.svg)](https://github.com/normdist-ai/china-mirrors/stargazers)
[![GitHub release](https://img.shields.io/github/v/release/normdist-ai/china-mirrors)](https://github.com/normdist-ai/china-mirrors/releases)

[中文版文档](README.zh.md)

Automatically configure domestic mirror sources for various package managers in China, solving dependency download speed issues for developers.

## 🚀 Features

- **11 Package Managers Supported**: pip, npm, yarn, pnpm, cargo, go mod, NuGet, RubyGems, Conda, Gradle, Homebrew
- **PDCA Testing**: Built-in script to test mirror availability and performance
- **Cross-Platform**: Windows, Linux, macOS
- **One-Click Config**: Automated scripts for global and project-level configuration

## ⚡ Quick Start

### One-Click Config All

```bash
cd skills/china-mirrors
python scripts/config_all.py --all
```

### Config Specific Package Manager

```bash
# Python pip - Aliyun mirror (default)
python scripts/config_all.py --pip

# Node.js npm - Tsinghua mirror
python scripts/config_all.py --npm tsinghua

# Go modules - Aliyun mirror (fastest)
python scripts/config_all.py --go aliyun
```

## 📦 Installation

### Option 1: One-Click Install (Recommended)

```bash
# Using npx (Node.js required)
npx skills add https://github.com/normdist-ai/china-mirrors --skill china-mirrors
```

### Option 2: Clone Manually

```bash
# Clone the repository
git clone https://github.com/normdist-ai/china-mirrors.git

# Copy the skill folder to your IDE's skills directory
cp -r china-mirrors/skills/china-mirrors ~/.config/opencode/skills/
```

### Option 3: For AI IDEs (OpenCode, Cursor, etc.)

Copy the `skills/china-mirrors` folder to your IDE's skills directory:

| IDE | Skills Directory |
|-----|-----------------|
| **OpenCode** | `~/.config/opencode/skills/china-mirrors` |
| **Cursor** | `.cursor/skills/china-mirrors` |
| **Lingma** | `.lingma/skills/china-mirrors` |
| **Trae** | `.trae/skills/china-mirrors` |

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

## 💻 CLI Reference

### config_all.py

```bash
python scripts/config_all.py [options]

Options:
  --pip [mirror]        Configure pip mirror (default: aliyun)
  --npm [mirror]        Configure npm mirror (default: aliyun)
  --cargo [mirror]      Configure cargo mirror (default: aliyun)
  --go [mirror]         Configure go mod mirror (default: aliyun)
  --nuget [mirror]      Configure NuGet mirror (default: huawei)
  --rubygems [mirror]   Configure RubyGems mirror (default: tsinghua)
  --conda [mirror]      Configure Conda mirror (default: tsinghua)
  --gradle [mirror]     Configure Gradle mirror (default: tencent)
  --homebrew [mirror]   Configure Homebrew mirror (default: ustc)
  --all, -a             Configure all detected tools
  --show, -s            Show available mirrors
  --timeout <seconds>   Request timeout (default: 10)
```

### test_mirrors.py

```bash
python scripts/test_mirrors.py [options]

Options:
  --type <types>        Test specific mirrors (e.g., pip npm)
  --verbose, -v         Show detailed output
  --save, -s            Save results to JSON
  --timeout <seconds>   Request timeout (default: 10)
```

## 📖 Usage Examples

### Example 1: Python Project

```bash
$ python scripts/config_all.py --pip

================================================================
  Configure Python pip
================================================================
✓ pip configuration saved to: C:\Users\xxx\pip\pip.ini
✓ Using mirror: Aliyun (https://mirrors.aliyun.com/pypi/simple/)

Now you can install dependencies:
  pip install -r requirements.txt
```

### Example 2: Multi-Language Project

```bash
$ python scripts/config_all.py --all

Start configuring mirrors...
Will configure 5 tools

================================================================
  Configure Python pip
================================================================
✓ pip configured: https://mirrors.aliyun.com/pypi/simple/

================================================================
  Configure Node.js package managers
================================================================
✓ npm configured: https://registry.npmmirror.com
✓ yarn configured: https://registry.npmmirror.com

...

================================================================
  Configuration Summary
================================================================
✓ Successfully configured:
  - pip
  - npm, yarn, pnpm
  - cargo
  - go
```

### Example 3: Test Mirror Availability

```bash
$ python scripts/test_mirrors.py --verbose

================================================================
  China Mirror Availability Test (PDCA)
================================================================
Test time: 2026-04-06 00:43:46
Timeout: 8s | Slow threshold: 3.0s

Testing Python (pip)...
  ✓ aliyun: 0.126s (HTTP 200)
  ✓ tsinghua: 0.182s (HTTP 200)
...

================================================================
  Test Results Summary
================================================================
Total: 33 | Passed: 24 | Slow: 1 | Failed: 9
Pass rate: 72.7%

================================================================
  Recommended Configuration
================================================================
✓ Python (pip): aliyun (0.126s)
✓ Node.js (npm): aliyun (0.114s)
...
```

## 📁 Project Structure

```
skills/
  china-mirrors/
    SKILL.md              # Skill definition & detailed guide
    README.md             # This file
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

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add some amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Ways to Contribute

- Report bugs
- Suggest new features
- Add support for more package managers
- Improve documentation
- Add more mirror sources
- Test on different platforms

## 📋 Related Documents

- [SKILL.md](skills/china-mirrors/SKILL.md) - Full skill documentation
- [QUICK_REFERENCE.md](skills/china-mirrors/QUICK_REFERENCE.md) - Quick reference
- [examples.md](skills/china-mirrors/examples.md) - More usage examples

## ⚠️ Known Limitations

- Docker Hub mirrors in China are mostly unavailable
- Flutter/Dart mirrors have moved to different paths
- Some mirrors may occasionally be slow or unavailable

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📮 Contact

- **GitHub Issues**: https://github.com/normdist-ai/china-mirrors/issues
- **Author**: normdist-ai
- **Email**: (open an issue for contact)

---

If you find this project helpful, please give it a ⭐ star!

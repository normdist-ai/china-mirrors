# 🇨🇳 China Mirrors - AI IDE Agent 技能

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/normdist-ai/china-mirrors.svg)](https://github.com/normdist-ai/china-mirrors/stargazers)

自动配置各种包管理器的国内镜像源，解决中国大陆开发者依赖下载缓慢的问题。

## 🚀 功能特性

- **支持 11 种包管理器**: pip、npm、yarn、pnpm、cargo、go mod、NuGet、RubyGems、Conda、Gradle、Homebrew
- **PDCA 测试**: 内置镜像可用性和性能测试脚本
- **跨平台**: Windows、Linux、macOS
- **一键配置**: 自动化脚本，支持全局和项目级配置

## 📦 安装

### AI IDE 使用（OpenCode、Cursor 等）

将 `skills/china-mirrors` 文件夹复制到你的 IDE 技能目录：

- **OpenCode**: `~/.config/opencode/skills/china-mirrors`
- **Cursor**: `.cursor/skills/china-mirrors`
- **Lingma**: `.lingma/skills/china-mirrors`

### 手动使用

```bash
cd skills/china-mirrors
python scripts/config_all.py --all
```

## 📊 镜像可用性测试

运行 PDCA 测试脚本验证镜像状态：

```bash
python scripts/test_mirrors.py --verbose
```

最新结果（2026-04-06）：**测试 33 个镜像，24 个可用**。

## 📁 项目结构

```
skills/
  china-mirrors/
    SKILL.md              # 技能定义和详细配置指南
    README.md             # 用户文档
    QUICK_REFERENCE.md    # 快速参考卡片
    examples.md           # 使用示例
    scripts/
      config_all.py       # 一键配置脚本
      config_pip.py       # pip 专用配置
      config_npm.js       # npm 专用配置
      test_mirrors.py     # PDCA 镜像可用性测试
    evals/
      evals.json          # 评估测试用例
```

## 📖 文档

完整文档请查看 [skills/china-mirrors/README.md](skills/china-mirrors/README.md)。

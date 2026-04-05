# 🇨🇳 China Mirrors - AI IDE Agent 技能

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/normdist-ai/china-mirrors.svg)](https://github.com/normdist-ai/china-mirrors/stargazers)
[![GitHub release](https://img.shields.io/github/v/release/normdist-ai/china-mirrors)](https://github.com/normdist-ai/china-mirrors/releases)

[English Documentation](README.md)

自动配置各种包管理器的国内镜像源，解决中国大陆开发者依赖下载缓慢的问题。

## 🚀 功能特性

- **支持 11 种包管理器**: pip、npm、yarn、pnpm、cargo、go mod、NuGet、RubyGems、Conda、Gradle、Homebrew
- **PDCA 测试**: 内置镜像可用性和性能测试脚本
- **跨平台**: Windows、Linux、macOS
- **一键配置**: 自动化脚本，支持全局和项目级配置

## ⚡ 快速开始

### 一键配置所有

```bash
cd skills/china-mirrors
python scripts/config_all.py --all
```

### 配置特定包管理器

```bash
# Python pip - 阿里云镜像（默认）
python scripts/config_all.py --pip

# Node.js npm - 清华大学镜像
python scripts/config_all.py --npm tsinghua

# Go modules - 阿里云镜像（最快）
python scripts/config_all.py --go aliyun
```

## 📦 安装

### 方式一：一键安装（推荐）

```bash
# 使用 npx（需要 Node.js）
npx skills add https://github.com/normdist-ai/china-mirrors --skill china-mirrors
```

### 方式二：手动克隆

```bash
# 克隆仓库
git clone https://github.com/normdist-ai/china-mirrors.git

# 复制技能文件夹到你的 IDE 技能目录
cp -r china-mirrors/skills/china-mirrors ~/.config/opencode/skills/
```

### 方式三：手动复制（AI IDE）

将 `skills/china-mirrors` 文件夹复制到你的 IDE 技能目录：

| IDE | 技能目录 |
|-----|---------|
| **OpenCode** | `~/.config/opencode/skills/china-mirrors` |
| **Cursor** | `.cursor/skills/china-mirrors` |
| **Lingma** | `.lingma/skills/china-mirrors` |
| **Trae** | `.trae/skills/china-mirrors` |

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

## 💻 CLI 参数参考

### config_all.py

```bash
python scripts/config_all.py [选项]

选项:
  --pip [镜像]        配置 pip 镜像（默认: aliyun）
  --npm [镜像]        配置 npm 镜像（默认: aliyun）
  --cargo [镜像]      配置 cargo 镜像（默认: aliyun）
  --go [镜像]         配置 go mod 镜像（默认: aliyun）
  --nuget [镜像]      配置 NuGet 镜像（默认: huawei）
  --rubygems [镜像]   配置 RubyGems 镜像（默认: tsinghua）
  --conda [镜像]      配置 Conda 镜像（默认: tsinghua）
  --gradle [镜像]     配置 Gradle 镜像（默认: tencent）
  --homebrew [镜像]   配置 Homebrew 镜像（默认: ustc）
  --all, -a           配置所有检测到的工具
  --show, -s          显示可用镜像列表
  --timeout <秒>      请求超时（默认: 10）
```

### test_mirrors.py

```bash
python scripts/test_mirrors.py [选项]

选项:
  --type <类型>       测试特定镜像（如: pip npm）
  --verbose, -v       显示详细输出
  --save, -s          保存结果到 JSON
  --timeout <秒>      请求超时（默认: 10）
```

## 📖 使用示例

### 示例 1: Python 项目

```bash
$ python scripts/config_all.py --pip

================================================================
  配置 Python pip
================================================================
✓ pip 配置已保存到: C:\Users\xxx\pip\pip.ini
✓ 使用镜像: 阿里云 (https://mirrors.aliyun.com/pypi/simple/)

现在可以安装依赖:
  pip install -r requirements.txt
```

### 示例 2: 多语言项目

```bash
$ python scripts/config_all.py --all

开始配置镜像源...
将配置 5 个工具

================================================================
  配置 Python pip
================================================================
✓ pip 配置成功: https://mirrors.aliyun.com/pypi/simple/

================================================================
  配置 Node.js 包管理器
================================================================
✓ npm 配置成功: https://registry.npmmirror.com
✓ yarn 配置成功: https://registry.npmmirror.com

...

================================================================
  配置完成摘要
================================================================
✓ 成功配置的工具:
  - pip
  - npm, yarn, pnpm
  - cargo
  - go
```

### 示例 3: 测试镜像可用性

```bash
$ python scripts/test_mirrors.py --verbose

================================================================
  中国国内镜像源可用性测试 (PDCA)
================================================================
测试时间: 2026-04-06 00:43:46
超时设置: 8秒 | 慢速阈值: 3.0秒

正在测试 Python (pip)...
  ✓ aliyun: 0.126s (HTTP 200)
  ✓ tsinghua: 0.182s (HTTP 200)
...

================================================================
  测试结果摘要
================================================================
总测试数: 33 | 通过: 24 | 慢速: 1 | 失败: 9
通过率: 72.7%

================================================================
  推荐配置
================================================================
✓ Python (pip): 阿里云 (0.126s)
✓ Node.js (npm): 阿里云 (0.114s)
...
```

## 📁 项目结构

```
skills/
  china-mirrors/
    SKILL.md              # 技能定义和详细配置指南
    README.md             # 英文文档
    README.zh.md          # 中文文档
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

## 🤝 贡献指南

欢迎贡献！以下是参与方式：

1. **Fork** 本仓库
2. **创建**功能分支 (`git checkout -b feature/amazing-feature`)
3. **提交**你的更改 (`git commit -m 'Add some amazing feature'`)
4. **推送**到分支 (`git push origin feature/amazing-feature`)
5. **打开** Pull Request

### 贡献方式

- 报告 bug
- 提出新功能建议
- 支持更多包管理器
- 改进文档
- 添加更多镜像源
- 在不同平台测试

## 📋 相关文档

- [SKILL.md](skills/china-mirrors/SKILL.md) - 完整技能文档
- [QUICK_REFERENCE.md](skills/china-mirrors/QUICK_REFERENCE.md) - 快速参考
- [examples.md](skills/china-mirrors/examples.md) - 更多使用示例

## ⚠️ 已知限制

- Docker Hub 国内镜像基本不可用
- Flutter/Dart 镜像已迁移到其他路径
- 部分镜像可能偶尔缓慢或不可用

## 📄 许可证

本项目基于 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 📮 联系方式

- **GitHub Issues**: https://github.com/normdist-ai/china-mirrors/issues
- **作者**: normdist-ai

---

如果本项目对你有帮助，请给一个 ⭐ star！

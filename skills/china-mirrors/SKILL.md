---
name: china-mirrors
description: 自动配置 Python pip、npm、yarn、pnpm、cargo、go mod、NuGet、RubyGems、Conda、Homebrew、Gradle 等包管理器的国内镜像源。使用当用户提到下载慢、安装依赖、配置镜像、加速包下载、设置国内源，或在中国大陆开发需要加速依赖安装时。支持阿里云、腾讯云、清华大学、中科大、华为云等主流镜像。检测到 package.json、requirements.txt、Cargo.toml、go.mod、Gemfile、.nuspec、environment.yml 等文件时主动建议使用此技能。
---

# 中国国内镜像源配置

为中国大陆开发者自动配置各种包管理器的国内镜像源，解决依赖下载缓慢问题。

## 快速开始

当用户遇到以下情况时使用此技能：
- 安装包依赖速度慢
- 提到"下载慢"、"超时"、"连接失败"
- 新建项目需要配置开发环境
- 检测到项目中有依赖配置文件（package.json、requirements.txt 等）

## 支持的包管理器

| 包管理器 | 配置文件 | 优先级 |
|---------|---------|--------|
| pip (Python) | requirements.txt, pyproject.toml | 高 |
| npm/yarn/pnpm (Node.js) | package.json | 高 |
| cargo (Rust) | Cargo.toml | 中 |
| go mod (Go) | go.mod | 中 |
| NuGet (.NET) | .csproj, packages.config | 中 |
| RubyGems (Ruby) | Gemfile | 中 |
| Conda (Python) | environment.yml | 中 |
| Gradle (Java/Kotlin) | build.gradle | 低 |
| Homebrew (macOS) | - | 低 |
| Maven (Java) | pom.xml | 低 |
| Composer (PHP) | composer.json | 低 |

## 推荐镜像源

> 以下推荐基于 2026-04-06 实测数据，详见 `scripts/mirror_test_results.json`。
> 可使用 `scripts/test_mirrors.py` 重新测试验证。

### Python (pip)
- **阿里云** ⭐ (0.028s): https://mirrors.aliyun.com/pypi/simple/
- **清华大学** (0.129s): https://pypi.tuna.tsinghua.edu.cn/simple/
- **腾讯云** (0.214s): https://mirrors.cloud.tencent.com/pypi/simple/
- **中科大** (0.359s): https://pypi.mirrors.ustc.edu.cn/simple/

### Node.js (npm/yarn/pnpm)
- **华为云** ⭐ (0.039s): https://repo.huaweicloud.com/repository/npm/
- **阿里云** (0.057s): https://registry.npmmirror.com
- **腾讯云** (0.492s): https://mirrors.cloud.tencent.com/npm/

### Rust (cargo)
- **阿里云** ⭐ (0.050s): https://mirrors.aliyun.com/crates.io-index/
- **清华大学** (0.084s): https://mirrors.tuna.tsinghua.edu.cn/crates.io-index/
- **中科大** (0.161s): https://mirrors.ustc.edu.cn/crates.io-index/

### Go (go mod)
- **阿里云** ⭐ (0.021s): https://mirrors.aliyun.com/goproxy/
- **七牛云** (0.086s): https://goproxy.cn
- **官方中国** (1.520s): https://goproxy.io

### Maven (Java)
- **阿里云** ⭐ (0.033s): https://maven.aliyun.com/repository/public

### Gradle (Java/Kotlin)
- **腾讯云** ⭐ (0.040s): https://mirrors.cloud.tencent.com/gradle/

### NuGet (.NET)
- **华为云** ⭐ (0.055s): https://repo.huaweicloud.com/repository/nuget/v3/index.json

### RubyGems (Ruby)
- **清华大学** ⭐ (0.129s): https://mirrors.tuna.tsinghua.edu.cn/rubygems/
- **中科大** (0.197s): https://mirrors.ustc.edu.cn/rubygems/

### Conda (Python)
- **清华大学** ⭐ (0.109s): https://mirrors.tuna.tsinghua.edu.cn/anaconda/
- **中科大** (0.381s): https://mirrors.ustc.edu.cn/anaconda/

### Homebrew (macOS)
- **中科大** ⭐ (0.231s): https://mirrors.ustc.edu.cn/homebrew-bottles/
- **清华大学** (0.592s): https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/

### Composer (PHP)
- **阿里云** ⭐ (0.049s): https://mirrors.aliyun.com/composer/

## 配置工作流程

### 步骤 1: 检测环境和需求

首先了解用户需求并检测环境：

1. **询问用户偏好**（如果未指定）：
   - "您希望配置哪些包管理器的镜像？"
   - "有偏好的镜像源吗？（默认推荐阿里云或清华大学）"

2. **检测操作系统**：
   ```bash
   # Windows
   echo $env:OS
   
   # Linux/Mac
   uname -s
   ```

3. **检测已安装的包管理器**：
    ```bash
    python --version
    node --version
    npm --version
    cargo --version
    go version
    dotnet --version
    ruby --version
    conda --version
    gradle --version
    brew --version
    ```

### 步骤 2: 选择配置方式

根据用户需求选择合适的配置方式：

**全局配置**（推荐）：一次配置，所有项目生效
**项目级配置**：仅当前项目生效，适合团队协作
**临时使用**：单次命令指定镜像，不修改配置

### 步骤 3: 执行配置

根据选择的包管理器执行相应配置（见下方详细配置指南）。

### 步骤 4: 验证配置

配置完成后，验证是否生效：

```bash
# pip
pip config list

# npm
npm config get registry

# cargo
cat ~/.cargo/config.toml

# go
go env GOPROXY

# nuget
dotnet nuget list source

# rubygems
gem sources -l

# conda
conda config --show channels

# gradle
gradle dependencies --info

# homebrew
brew config | grep HOMEBREW_BOTTLE_DOMAIN
```

### 步骤 5: 提供使用说明

告诉用户：
- 如何恢复默认配置
- 如何在特定命令中临时使用其他镜像
- 常见问题排查方法

---

## 详细配置指南

### Python (pip) 配置

#### 方法 1: 全局配置（推荐）

**Windows:**
```powershell
# 创建配置文件目录
mkdir ~\pip

# 创建 pip.ini 文件
@"
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/
trusted-host = mirrors.aliyun.com

[install]
trusted-host = mirrors.aliyun.com
"@ | Out-File -FilePath ~\pip\pip.ini -Encoding utf8
```

**Linux/Mac:**
```bash
mkdir -p ~/.pip
cat > ~/.pip/pip.conf << EOF
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/
trusted-host = mirrors.aliyun.com

[install]
trusted-host = mirrors.aliyun.com
EOF
```

#### 方法 2: 项目级配置

在项目根目录创建 `setup.cfg` 或 `pyproject.toml`：

**setup.cfg:**
```ini
[easy_install]
index_url = https://mirrors.aliyun.com/pypi/simple/
```

**pyproject.toml:**
```toml
[[tool.poetry.source]]
name = "aliyun"
url = "https://mirrors.aliyun.com/pypi/simple/"
priority = "primary"
```

#### 方法 3: 临时使用

```bash
pip install -i https://mirrors.aliyun.com/pypi/simple/ package_name
```

#### 验证配置
```bash
pip config list
pip install --verbose some_package 2>&1 | grep -i "aliyun\|tsinghua"
```

---

### npm/yarn/pnpm 配置

#### 方法 1: 全局配置（推荐）

**npm:**
```bash
npm config set registry https://registry.npmmirror.com
```

**yarn:**
```bash
yarn config set registry https://registry.npmmirror.com
```

**pnpm:**
```bash
pnpm config set registry https://registry.npmmirror.com
```

#### 方法 2: 项目级配置

在项目根目录创建 `.npmrc` 文件：

```ini
registry=https://registry.npmmirror.com
```

#### 方法 3: 临时使用

```bash
npm install --registry=https://registry.npmmirror.com package_name
```

#### 验证配置
```bash
npm config get registry
# 应输出: https://registry.npmmirror.com
```

---

### Rust (cargo) 配置

#### 方法 1: 全局配置（推荐）

**Windows/Linux/Mac:**

创建或编辑 `~/.cargo/config.toml`：

```toml
[source.crates-io]
replace-with = 'aliyun'

[source.aliyun]
registry = "https://mirrors.aliyun.com/crates.io-index/"

# 或使用清华大学
# [source.tuna]
# registry = "https://mirrors.tuna.tsinghua.edu.cn/crates.io-index/"
```

#### 方法 2: 项目级配置

在项目根目录创建 `.cargo/config.toml`：

```toml
[source.crates-io]
replace-with = 'aliyun'

[source.aliyun]
registry = "https://mirrors.aliyun.com/crates.io-index/"
```

#### 验证配置
```bash
cargo search serde --limit 1
# 观察是否从清华镜像下载索引
```

---

### Go (go mod) 配置

#### 方法 1: 环境变量配置（推荐）

**Windows PowerShell:**
```powershell
# 永久设置
[Environment]::SetEnvironmentVariable("GOPROXY", "https://mirrors.aliyun.com/goproxy/,https://goproxy.cn,direct", "User")
[Environment]::SetEnvironmentVariable("GONOSUMDB", "*", "User")

# 立即生效
$env:GOPROXY = "https://mirrors.aliyun.com/goproxy/,https://goproxy.cn,direct"
$env:GONOSUMDB = "*"
```

**Linux/Mac (bash/zsh):**
```bash
# 添加到 ~/.bashrc 或 ~/.zshrc
echo 'export GOPROXY=https://mirrors.aliyun.com/goproxy/,https://goproxy.cn,direct' >> ~/.bashrc
echo 'export GONOSUMDB=*' >> ~/.bashrc
source ~/.bashrc
```

#### 方法 2: 临时使用

```bash
GOPROXY=https://mirrors.aliyun.com/goproxy/,https://goproxy.cn,direct go get package_name
```

#### 验证配置
```bash
go env GOPROXY
# 应输出: https://mirrors.aliyun.com/goproxy/,https://goproxy.cn,direct
```

---

### Maven (Java) 配置

编辑 `~/.m2/settings.xml`（不存在则创建）：

```xml
<settings>
  <mirrors>
    <mirror>
      <id>aliyun</id>
      <mirrorOf>central</mirrorOf>
      <name>Aliyun Maven</name>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
  </mirrors>
</settings>
```

---

### Composer (PHP) 配置

```bash
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
```

---

### NuGet (.NET) 配置

#### 方法 1: 全局配置（推荐）

**Windows PowerShell:**
```powershell
# 创建 NuGet 配置文件目录
$nugetConfigDir = Join-Path $env:APPDATA "NuGet"
New-Item -ItemType Directory -Force -Path $nugetConfigDir

# 创建 NuGet.Config
@"
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <clear />
    <add key="huawei" value="https://repo.huaweicloud.com/repository/nuget/v3/index.json" />
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
  </packageSources>
</configuration>
"@ | Out-File -FilePath (Join-Path $nugetConfigDir "NuGet.Config") -Encoding utf8
```

**Linux/Mac:**
```bash
mkdir -p ~/.nuget
cat > ~/.nuget/NuGet.Config << 'EOF'
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <clear />
    <add key="huawei" value="https://repo.huaweicloud.com/repository/nuget/v3/index.json" />
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
  </packageSources>
</configuration>
EOF
```

#### 方法 2: 项目级配置

在项目根目录创建 `nuget.config`：
```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <clear />
    <add key="huawei" value="https://repo.huaweicloud.com/repository/nuget/v3/index.json" />
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
  </packageSources>
</configuration>
```

#### 验证配置
```bash
dotnet nuget list source
```

---

### RubyGems (Ruby) 配置

#### 方法 1: 全局配置（推荐）

```bash
# 移除默认源
gem sources --remove https://rubygems.org/

# 添加清华镜像
gem sources -a https://mirrors.tuna.tsinghua.edu.cn/rubygems/

# 验证
gem sources -l
```

#### 方法 2: 项目级配置

在项目根目录创建 `.gemrc`：
```yaml
---
sources:
- https://mirrors.tuna.tsinghua.edu.cn/rubygems/
- https://rubygems.org/
```

#### 方法 3: Bundler 配置

```bash
bundle config mirror.https://rubygems.org https://mirrors.tuna.tsinghua.edu.cn/rubygems/
```

#### 验证配置
```bash
gem sources -l
bundle config mirror.https://rubygems.org
```

---

### Conda (Python) 配置

#### 方法 1: 全局配置（推荐）

**Windows/Linux/Mac:**
```bash
# 添加清华镜像
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --set show_channel_urls yes
```

#### 方法 2: 配置文件

编辑 `~/.condarc`（不存在则创建）：
```yaml
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

#### 验证配置
```bash
conda config --show channels
conda install numpy  # 观察是否从清华镜像下载
```

---

### Gradle (Java/Kotlin) 配置

#### 方法 1: 项目级配置（推荐）

在项目 `build.gradle` 或 `build.gradle.kts` 中修改：

**Groovy DSL (build.gradle):**
```groovy
repositories {
    maven { url 'https://maven.aliyun.com/repository/public/' }
    maven { url 'https://mirrors.cloud.tencent.com/gradle/' }
    mavenCentral()
}
```

**Kotlin DSL (build.gradle.kts):**
```kotlin
repositories {
    maven("https://maven.aliyun.com/repository/public/")
    maven("https://mirrors.cloud.tencent.com/gradle/")
    mavenCentral()
}
```

#### 方法 2: 全局配置

编辑 `~/.gradle/init.gradle` 或 `~/.gradle/init.gradle.kts`：

**Groovy DSL:**
```groovy
allprojects {
    repositories {
        def ALIYUN = 'https://maven.aliyun.com/repository/public/'
        def TENCENT = 'https://mirrors.cloud.tencent.com/gradle/'
        all { ArtifactRepository repo ->
            if (repo instanceof MavenArtifactRepository) {
                def url = repo.url.toString()
                if (url.startsWith('https://repo.maven.apache.org/maven2') ||
                    url.startsWith('https://jcenter.bintray.com/')) {
                    project.logger.lifecycle "Repository ${repo.url} replaced by aliyun."
                    remove repo
                }
            }
        }
        maven { url ALIYUN }
        maven { url TENCENT }
    }
}
```

#### 方法 3: Gradle Wrapper 下载加速

**Windows PowerShell:**
```powershell
$env:GRADLE_OPTS = "-Dgradle.wrapperBase=https://mirrors.cloud.tencent.com/gradle/"
```

**Linux/Mac:**
```bash
export GRADLE_OPTS="-Dgradle.wrapperBase=https://mirrors.cloud.tencent.com/gradle/"
```

#### 验证配置
```bash
gradle dependencies --info | grep -i "aliyun\|tencent"
```

---

### Homebrew (macOS) 配置

#### 方法 1: 环境变量配置

编辑 `~/.zshrc` 或 `~/.bashrc`：
```bash
# Homebrew Bottles 中科大镜像
export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles/
```

使配置生效：
```bash
source ~/.zshrc  # 或 source ~/.bashrc
```

#### 方法 2: 替换 Homebrew 仓库镜像

```bash
# 替换 homebrew-core
cd "$(brew --repo homebrew/core)"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

# 替换 homebrew-cask
cd "$(brew --repo homebrew/cask)"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git

# 更新
brew update
```

#### 验证配置
```bash
brew config | grep HOMEBREW_BOTTLE_DOMAIN
brew install wget  # 观察是否从镜像下载
```

#### 恢复默认配置
```bash
cd "$(brew --repo homebrew/core)"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git
cd "$(brew --repo homebrew/cask)"
git remote set-url origin https://github.com/Homebrew/homebrew-cask.git
unset HOMEBREW_BOTTLE_DOMAIN
```

---

## 自动化配置脚本

### Python 自动配置脚本

创建 `scripts/config_pip.py`：

```python
#!/usr/bin/env python3
"""自动配置 pip 国内镜像源"""
import os
import sys
import platform
from pathlib import Path

MIRRORS = {
    'aliyun': 'https://mirrors.aliyun.com/pypi/simple/',
    'tsinghua': 'https://pypi.tuna.tsinghua.edu.cn/simple/',
    'ustc': 'https://pypi.mirrors.ustc.edu.cn/simple/',
}

def get_config_path():
    """获取 pip 配置文件路径"""
    system = platform.system()
    if system == 'Windows':
        return Path.home() / 'pip' / 'pip.ini'
    else:
        return Path.home() / '.pip' / 'pip.conf'

def create_config(mirror='aliyun'):
    """创建 pip 配置文件"""
    config_path = get_config_path()
    config_path.parent.mkdir(parents=True, exist_ok=True)
    
    mirror_url = MIRRORS.get(mirror, MIRRORS['aliyun'])
    host = mirror_url.split('/')[2]
    
    config_content = f"""[global]
index-url = {mirror_url}
trusted-host = {host}

[install]
trusted-host = {host}
"""
    
    config_path.write_text(config_content, encoding='utf-8')
    print(f"✓ pip 配置已保存到: {config_path}")
    print(f"✓ 使用镜像: {mirror_url}")

if __name__ == '__main__':
    mirror = sys.argv[1] if len(sys.argv) > 1 else 'aliyun'
    create_config(mirror)
```

使用方法：
```bash
python scripts/config_pip.py aliyun
```

### Node.js 自动配置脚本

创建 `scripts/config_npm.js`：

```javascript
#!/usr/bin/env node
/**
 * 自动配置 npm/yarn/pnpm 国内镜像源
 */
const { execSync } = require('child_process');

const MIRROR = 'https://registry.npmmirror.com';

function detectPackageManager() {
    const managers = [];
    
    try {
        execSync('npm --version', { stdio: 'ignore' });
        managers.push('npm');
    } catch (e) {}
    
    try {
        execSync('yarn --version', { stdio: 'ignore' });
        managers.push('yarn');
    } catch (e) {}
    
    try {
        execSync('pnpm --version', { stdio: 'ignore' });
        managers.push('pnpm');
    } catch (e) {}
    
    return managers;
}

function configure(manager) {
    console.log(`正在配置 ${manager}...`);
    try {
        execSync(`${manager} config set registry ${MIRROR}`, { stdio: 'inherit' });
        console.log(`✓ ${manager} 配置成功`);
    } catch (error) {
        console.error(`✗ ${manager} 配置失败:`, error.message);
    }
}

// 主程序
const managers = detectPackageManager();
if (managers.length === 0) {
    console.log('未检测到任何包管理器');
    process.exit(1);
}

console.log('检测到包管理器:', managers.join(', '));
managers.forEach(configure);
```

使用方法：
```bash
node scripts/config_npm.js
```

---

## 镜像可用性测试

本技能包含 PDCA 循环测试脚本，用于验证各镜像源的可用性和性能。

### 运行测试

```bash
# 测试所有镜像
python scripts/test_mirrors.py --verbose

# 仅测试特定类型
python scripts/test_mirrors.py --type pip npm

# 保存结果到 JSON
python scripts/test_mirrors.py --save

# 自定义超时时间
python scripts/test_mirrors.py --timeout 5
```

### 测试结果解读

- **✓ 可用**: 响应正常且速度 < 3秒
- **⚠ 慢速**: 响应正常但速度 > 3秒
- **✗ 错误/不可达**: 无法连接或返回错误

### 最新测试结果

详见 `scripts/mirror_test_results.json`（2026-04-06 测试，15/15 通过率 100%）。

---

## 常见问题排查

### pip 配置后仍然很慢

**问题**: 配置了镜像但仍从官方源下载

**解决**:
1. 检查配置文件是否正确：
   ```bash
   pip config list
   ```
2. 清除缓存重试：
   ```bash
   pip cache purge
   pip install package_name
   ```
3. 使用 `-v` 参数查看详细日志：
   ```bash
   pip install -v package_name
   ```

### npm 配置后报错 SSL 错误

**问题**: CERT_HAS_EXPIRED 或其他 SSL 错误

**解决**:
```bash
# 临时禁用严格 SSL（不推荐长期使用）
npm config set strict-ssl false

# 或更新证书
npm config delete cafile
```

### cargo 配置后编译失败

**问题**: crates.io 索引同步问题

**解决**:
```bash
# 清除 cargo 缓存
cargo clean
rm -rf ~/.cargo/registry

# 重新构建
cargo build
```

### go mod 配置后无法下载私有仓库

**问题**: GOPROXY 拦截了私有仓库

**解决**:
```bash
# 设置私有仓库不走代理
export GOPRIVATE=github.com/your-org/*
export GONOSUMDB=github.com/your-org/*
```

---

## 恢复默认配置

### pip
```bash
# 删除配置文件
rm ~/.pip/pip.conf          # Linux/Mac
del %USERPROFILE%\pip\pip.ini  # Windows
```

### npm
```bash
npm config delete registry
```

### cargo
```bash
# 注释或删除 ~/.cargo/config.toml 中的镜像配置
```

### go
```bash
unset GOPROXY  # Linux/Mac
# Windows: 通过系统环境变量设置界面删除
```

### nuget
```bash
# 删除配置文件
rm ~/.nuget/NuGet.Config          # Linux/Mac
del %APPDATA%\NuGet\NuGet.Config  # Windows
```

### rubygems
```bash
gem sources --remove https://mirrors.tuna.tsinghua.edu.cn/rubygems/
gem sources -a https://rubygems.org/
```

### conda
```bash
conda config --remove-key channels
```

### gradle
# 删除 ~/.gradle/init.gradle 或修改 build.gradle 恢复默认仓库

### homebrew
```bash
unset HOMEBREW_BOTTLE_DOMAIN
cd "$(brew --repo homebrew/core)" && git remote set-url origin https://github.com/Homebrew/homebrew-core.git
cd "$(brew --repo homebrew/cask)" && git remote set-url origin https://github.com/Homebrew/homebrew-cask.git
```

---

## 最佳实践

1. **优先使用阿里云或清华大学镜像**：稳定性好，更新及时
2. **团队项目使用项目级配置**：在版本控制中包含 `.npmrc` 或 `pip.conf`
3. **CI/CD 环境中显式指定镜像**：避免依赖全局配置
4. **定期检查镜像可用性**：某些镜像可能暂时不可用
5. **保留回退方案**：知道如何快速切换到其他镜像或官方源

## 相关资源

- [PyPI 镜像列表](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)
- [npm 镜像状态](https://npmmirror.com/)
- [crates.io 镜像](https://mirrors.tuna.tsinghua.edu.cn/help/crates.io-index/)
- [Go Proxy 中国](https://goproxy.cn/)

# @4xian/ccapi

[English](./README_EN.md) | 中文

Claude Code settings.json中key自动配置工具，方便API_KEY、AUTH_TOKEN以及多Model之间快速切换

## 功能特性

- 🚀 **一键切换** - 轻松在不同 Claude API 配置间切换
- 🔒 **安全备份** - 修改前自动备份 settings.json 文件
- 📝 **友好提示** - 详细的错误信息和操作指导
- 🎯 **智能识别** - 自动识别当前使用的配置
- 🛡️ **数据保护** - 敏感信息脱敏显示

## 安装

### 全局安装（推荐）

```bash
npm install -g @4xian/ccapi
```

## 使用方法

### 1. 查看版本

```bash
ccapi -v
```

### 2. 设置配置文件路径

初次使用需要设置 Claude Code 的 settings.json 文件路径和自定义API配置文件路径：

```bash
例如:
# 同时设置两个路径
ccapi set --settings /Users/4xian/.claude/settings.json --api /Users/4xian/Desktop/api.json

# 或分别设置
ccapi set --settings /Users/4xian/.claude/settings.json
ccapi set --api /Users/4xian/Desktop/api.json

# 查询当前配置文件路径
ccapi set
```

### 3. 自定义API配置文件格式

创建一个`api.json`文件，格式如下：

```json
{
  "openrouter": {
    "url": "xxx",
    "token": "your-auth-token",
    "model": "claude-sonnet-4-20250514",
    "fast": "claude-3-5-haiku-20241022",
    "timeout": 120000,
    "tokens": 20000
  },
  "multimodel": {
    "url": "https://api.example.com",
    "key": "your-api-key",
    "model": [
      "claude-sonnet-4-20250514",
      "claude-3-5-haiku-20241022",
      "claude-3-opus-20240229"
    ],
    "fast": [
      "claude-3-5-haiku-20241022",
      "claude-3-haiku-20240307"
    ]
  }
}
```

**字段说明：**
【不同厂商提供的可能是key, 也可能是token, 若不能使用可将key和token互换一下】
【本工具只支持Anthropic格式的配置, 当然只要Claude能用就都可以】

- `url`: API厂商服务器地址（必需）
- `key`: API_KEY（key 和 token 至少需要一个）
- `token`: AUTH_TOKEN（key 和 token 至少需要一个）
- `model`: 模型名称（非必需，默认claude-sonnet-4-20250514）
  - **字符串格式**: 直接指定一个模型
  - **数组格式**: 可指定多个模型，支持通过索引切换
- `fast`: 快速模型名称（非必需，默认claude-3-5-haiku-20241022）
  - **字符串格式**: 直接指定一个快速模型
  - **数组格式**: 可指定多个快速模型，支持通过索引切换
- `timeout`: 请求超时时间（非必需，默认600000ms）
- `tokens`: 最大输出令牌数（非必需，默认25000）
- `http`: 为网络连接指定 HTTP 代理服务器
- `https`: 为网络连接指定 HTTPS 代理服务器

### 4. 列举api配置文件中的可用配置

```bash
ccapi ls 或者 ccapi list
```

显示效果：

```text
可用的API配置:

  【openrouter】
    URL: https://api.openrouter.ai
    Model: claude-sonnet-4-20250514
    Fast: claude-3-5-haiku-20241022
    Key: sk-or123...

* 【multimodel】
    URL: https://api.example.com
    Model:
    * - 1: claude-sonnet-4-20250514
      - 2: claude-3-5-haiku-20241022
      - 3: claude-3-opus-20240229
    Fast:
      - 1: claude-3-5-haiku-20241022
    * - 2: claude-3-haiku-20240307
    Key: sk-abc123...
```

**显示说明：**

- 带`*`号的配置表示当前正在使用
- 对于数组格式的 model/fast，会显示索引编号

### 5. 自由切换配置(切换成功后记得重启Claude终端才会生效!!!)

#### 基本切换

```bash
# 切换到指定配置（使用默认模型）
ccapi use openrouter

# 对于字符串格式的 model/fast，直接切换
ccapi use anyrouter
```

#### 高级切换（适用于数组格式）

```bash
# 切换到 multimodel 配置的第2个模型和第1个快速模型
ccapi use multimodel -m 2 -f 1

# 只指定标准模型索引
ccapi use multimodel -m 3

# 只指定快速模型索引
ccapi use multimodel -f 2
```

**参数说明：**

- `-m <index>`: 指定要使用的模型索引（从1开始计数）
- `-f <index>`: 指定要使用的快速模型索引（从1开始计数）
- 对于字符串格式的配置，会自动忽略索引参数
- 不指定索引时默认使用数组的第一个元素

## 系统要求

- Node.js >= 14.0.0
- 支持的操作系统: macOS, Linux, Windows

# @4xian/ccapi

[English](./README_EN.md) | 中文

Claude Code settings.json中key自动配置工具，方便多个API_KEY、AUTH_TOKEN以及多Model之间快速切换

## 功能特性

- 🚀 **一键切换** - 轻松在不同 Claude API 配置间切换
- ⚡ **延迟测试** - 快速同时测试所有API配置，延迟智能排序显示
- 🎯 **自动优选** - 自动测试并切换到延迟最低的最优配置
- 🔒 **安全备份** - 修改前自动备份 settings.json 文件
- 🧠 **智能识别** - 自动识别当前使用的配置和最优路线
- 📄 **多格式支持** - 支持 JSON、JSON5、YAML、TOML 配置文件
- 🔧 **数组支持** - 支持多个URL、Key、Token、Model等字段的数组配置

## 安装

### 全局安装（推荐）

```bash
# 安装
npm install -g @4xian/ccapi

# 卸载
npm uninstall -g @4xian/ccapi
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
ccapi set --settings ~/.claude/settings.json --api /Users/4xian/Desktop/api.json5

# 分别设置
ccapi set --settings ~/.claude/settings.json
ccapi set --api /Users/4xian/Desktop/api.json5

# 直接在配置文件中修改路径
在 ~/.ccapi-config.json 文件中(与.claude同级)，有存储路径的变量，直接修改即可
  {
    "settingsPath": "~/.claude/settings.json",
    "apiConfigPath": "/Users/4xian/Desktop/api.json5",
  }

# 查询当前配置文件路径
ccapi set
```

### 3. 自定义API配置文件格式

支持多种配置文件格式：**JSON、JSON5、YAML、TOML**
创建一个配置文件（如 `api.json`、`api.yaml`、`api.json5` 或 `api.toml`），格式如下：

**JSON 格式示例：**

```json5
{
  "openrouter": {
    "url": "https://api.openrouter.ai",
    "token": "your-auth-token",
    "model": "claude-sonnet-4-20250514",
    "fast": "claude-3-5-haiku-20241022",
    "timeout": 600000,
    "tokens": 65000
  },
  // 推荐使用数组形式，可进行多个配置
  "multiconfig": {
    "url": [
      "https://api.example1.com",
      "https://api.example2.com"
    ],
    "token": [
      "token1-for-auth",
      "token2-for-auth"
    ],
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

**YAML 格式示例：**

```yaml
openrouter:
  url: "https://api.openrouter.ai"
  token: "your-auth-token"
  model: "claude-sonnet-4-20250514"
  fast: "claude-3-5-haiku-20241022"
  timeout: 600000
  tokens: 65000

multiconfig:
  url:
    - "https://api.example1.com"
    - "https://api.example2.com"
  token:
    - "token1-for-auth"
    - "token2-for-auth"
  model:
    - "claude-sonnet-4-20250514"
    - "claude-3-5-haiku-20241022"
    - "claude-3-opus-20240229"
  fast:
    - "claude-3-5-haiku-20241022"
    - "claude-3-haiku-20240307"
```

**JSON5 格式示例（支持注释）：**

```json5
{
  // OpenRouter 配置
  "openrouter": {
    "url": "https://api.openrouter.ai",
    "token": "your-auth-token",
    "model": "claude-sonnet-4-20250514",  // 默认模型
    "fast": "claude-3-5-haiku-20241022",  // 快速模型
    "timeout": 600000,  // 请求超时时间
    "tokens": 65000  // 最大输出令牌数
  }
}
```

**TOML 格式示例：**

```toml
[openrouter]
url = "https://api.openrouter.ai"
token = "your-auth-token"
model = "claude-sonnet-4-20250514"
fast = "claude-3-5-haiku-20241022"
timeout = 600000
tokens = 65000

[multiconfig]
url = [
  "https://api.example1.com",
  "https://api.example2.com"
]
token = [
  "token1-for-auth",
  "token2-for-auth"
]
model = [
  "claude-sonnet-4-20250514",
  "claude-3-5-haiku-20241022",
  "claude-3-opus-20240229"
]
fast = [
  "claude-3-5-haiku-20241022",
  "claude-3-haiku-20240307"
]
```

**字段说明：**
【不同厂商提供的可能是key, 也可能是token, 若不能使用可将key和token互换一下】
【本工具只提供快速切换环境变量的功能，因此只支持Anthropic格式的配置, 当然只要Claude Code能用就都可以】

- `url`: API厂商服务器地址（必需）
  - **字符串格式**: 直接指定一个URL
  - **数组格式**: 可指定多个URL，支持通过索引切换
- `key`: API_KEY（key 和 token 同时只需要一个）
  - **字符串格式**: 直接指定一个API Key
  - **数组格式**: 可指定多个API Key，支持通过索引切换
- `token`: AUTH_TOKEN（key 和 token 同时只需要一个）
  - **字符串格式**: 直接指定一个Auth Token
  - **数组格式**: 可指定多个Auth Token，支持通过索引切换
- `model`: 模型名称（非必需，默认claude-sonnet-4-20250514）
  - **字符串格式**: 直接指定一个模型
  - **数组格式**: 可指定多个模型，支持通过索引切换
- `fast`: 快速模型名称（非必需，默认claude-3-5-haiku-20241022）
  - **字符串格式**: 直接指定一个快速模型
  - **数组格式**: 可指定多个快速模型，支持通过索引切换
- `timeout`: 请求超时时间（非必需，默认为官方600000ms）
- `tokens`: 最大输出令牌数（非必需，默认以官方为准）
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
    Token: your-auth-token...

* 【multiconfig】
    URL:
    * - 1: https://api.example1.com
      - 2: https://api.example2.com
    Model:
    * - 1: claude-sonnet-4-20250514
      - 2: claude-3-5-haiku-20241022
      - 3: claude-3-opus-20240229
    Fast:
      - 1: claude-3-5-haiku-20241022
    * - 2: claude-3-haiku-20240307
    Key:
    * - 1: sk-key1-for-api1...
      - 2: sk-key2-for-api2...
    Token:
      - 1: token1-for-auth...
    * - 2: token2-for-auth...
```

**显示说明：**

- 带`*`号的配置表示当前正在使用
- 对于数组格式的 url/key/token/model/fast，会显示索引编号
- 当前使用的项会用 `* - ` 标识并高亮显示
- 敏感信息（key、token）会自动脱敏显示

### 5. 自由切换配置(切换成功后记得重启Claude终端才会生效!!!)

#### 基本切换

```bash
# 切换到指定配置（使用默认模型，配置若为数组，则默认使用第一个）
ccapi use openrouter

# 对于字符串格式的 model/fast，直接切换
ccapi use anyrouter
```

#### 高级切换（适用于数组格式）

```bash
# 切换到 multiconfig 配置，并使用第一个url,第一个token,第2个模型，第1个快速模型
ccapi use multiconfig -u 1 -t 1 -m 2 -f 1

# 只切换某些字段的索引
ccapi use multiconfig -k 1      # 只切换到某个Key
ccapi use multiconfig -t 2      # 只切换到某个Token
ccapi use multiconfig -u 1      # 只切换到某个URL
ccapi use multiconfig -m 3      # 只切换到某个Model
ccapi use multiconfig -f 2      # 只切换到某个Fast Model索引
```

**参数说明：**

- `-u <index>`: 指定要使用的URL索引（从1开始计数）
- `-k <index>`: 指定要使用的Key索引（从1开始计数）
- `-t <index>`: 指定要使用的Token索引（从1开始计数）
- `-m <index>`: 指定要使用的模型索引（从1开始计数）
- `-f <index>`: 指定要使用的快速模型索引（从1开始计数）
- 对于字符串格式的配置，会自动忽略索引参数
- 不指定索引时默认使用数组的第一个元素
- 可以任意组合使用这些参数

### 6. 测试API延迟

#### 测试所有配置（默认并行模式）

```bash
# 并行测试（快速，推荐）
ccapi test

# 串行测试（逐一测试）
ccapi test -s
```

#### 测试指定配置

```bash
# 并行测试指定配置
ccapi test openrouter

# 串行测试指定配置
ccapi test -s openrouter
```

**测试模式说明：**

- **并行测试（默认）**: 同时测试所有URL，速度快，结果按配置的最佳延迟排序显示
- **串行测试（-s选项）**: 逐个测试URL，按配置分组显示

**测试说明：**

- **测试超时时间**：默认为5秒，可在 ~/.ccapi-config.json 文件中新增变量控制超时，如：testTimeout: 10000
- **测试返回的结果**：默认不显示，由于厂商不同，返回结果仅供参考，返回错误不代表不能在claude code中使用，只需关注延迟时间就行，可在 ~/.ccapi-config.json 文件中新增变量是否显示结果，如：testResponse: true

  ```json5
  {
    "settingsPath": "~/.claude/settings.json",
    "apiConfigPath": "/Users/4xian/Desktop/api.json5",
    "testTimeout": 5000,
    "testResponse": false
  }
  ```

- 对于数组格式的url，会测试所有URL地址，数组配置的url内部不会按延迟排序，保持原有的url顺序
- 配置按最佳延迟排序，延迟最低的配置排在前面
- 显示每个配置的最优路线（最快的URL地址）

**并行测试结果示例：**

```text
测试结果(按延迟从低到高): 

【xxx】(最优路线: xxx/claude)
    1.[https://xxx/claude] ● 628ms 

【multiconfig】(最优路线: api.example1.com)
    1.[https://api.example1.com] ● 856ms 
    2.[https://api.example2.com] ● 892ms 

```

### 7. 自动选择最优配置

#### 基本自动选择

```bash
# 会先进行测试，然后选择最优的配置进行自动切换
ccapi auto
```

#### 多命令配合执行

```bash
# 常用于组合命令，这样每次启动claude前都会选择最优路线
ccapi auto && claude

# 也可以自定义别名，每次使用别名启动
alias cc=ccapi auto && claude
cc
```

**功能说明：**

- **使用并行测试**：快速同时测试所有API配置的延迟
- **智能选择**：自动选择全局延迟最低的配置并切换
- **注意事项**：
  - 对于数组格式的配置，自动选择最优URL
  - 若KEY/TOKEN为数组，则会对齐最优URL索引进行搭配，比如：最优URL为索引1，KEY/TOKEN也会选择索引1，最优URL为2，KEY/TOKEN也会选择索引2，若不想自动切换KEY/TOKEN，将其始终配为一个即可
- **趣味搭配**：
  - 由于自动配置的对齐规则，可以在一个配置中进行多厂商配置，比如：

    ```json5
      {
        "aaa": {
          "url": [
            "https: 第一个厂商.com",
            "https: 第二个厂商.com",
            "https: 第三个厂商.com",
          ],
          "token": [
            "第一个厂商的token",
            "第二个厂商的token",
            "第三个厂商的token",
          ],
          "model": ["xxx"]
        },
        "bbb": {
          "url": [
            "https: 第一个厂商.com",
            "https: 第二个厂商.com",
            "https: 第三个厂商.com",
          ],
          "key": [
            "第一个厂商的key",
            "第二个厂商的key",
            "第三个厂商的key",
          ],
          "model": ["xxx"]
        },
      }
    ```

    - 这样自动选择第一个厂商的同时会自动选择第一个厂商的token，选择第二个厂商的同时会自动选择第二个厂商的token...
    - 注意token类的厂商放一起，key类的厂商放一起

## 系统要求

- Node.js >= 14.0.0
- 支持的操作系统: macOS, Linux, Windows

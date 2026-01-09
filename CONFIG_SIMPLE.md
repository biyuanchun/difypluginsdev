# 简化配置说明

## 📋 配置方式改进

现在插件支持**两种配置方式**：

### 方式 1: 表单字段配置（推荐，简单易用）

不再需要手动输入 JSON，只需填写独立的表单字段：

#### LLM 配置
- **LLM Provider** (必需，默认: `azure_openai`): `azure_openai` | `openai` | `anthropic` 等
  - 推荐使用 `azure_openai`
- **LLM Model** (必需): 例如 `gpt-4o-mini`
- **LLM API Key** (条件必需): 
  - ✅ 当 Provider 为 `openai` 或 `azure_openai` 时需要
  - ❌ 当 Provider 为 `anthropic` 时不需要
- **Azure Endpoint** (条件必需):
  - ✅ 仅当 Provider 为 `azure_openai` 时需要
  - ❌ 其他 Provider 可留空
- **Azure Deployment** (条件必需):
  - ✅ 仅当 Provider 为 `azure_openai` 时需要
  - ❌ 其他 Provider 可留空
- **LLM Temperature** (可选): 温度参数（默认 `0.1`）
- **LLM Max Tokens** (可选): 最大 Token 数（默认 `256`）

#### Embedder 配置
- **Embedder Provider** (必需): `openai` | `azure_openai` | `huggingface`
- **Embedder Model** (必需): 例如 `text-embedding-3-small`
- **Embedder API Key** (条件必需):
  - ✅ 当 Provider 为 `openai` 或 `azure_openai` 时需要
  - ❌ 当 Provider 为 `huggingface` 时不需要
- **Embedder Azure Endpoint** (条件必需):
  - ✅ 仅当 Provider 为 `azure_openai` 时需要
  - ❌ 其他 Provider 可留空
- **Embedder Azure Deployment** (条件必需):
  - ✅ 仅当 Provider 为 `azure_openai` 时需要
  - ❌ 其他 Provider 可留空

#### Vector Database 配置
- **Vector DB Provider**: `pgvector`（默认）
- **Database Host**: 例如 `localhost`
- **Database Port**: 例如 `5432`（默认）
- **Database Name**: 例如 `mem0_vectors`
- **Database User**: 例如 `postgres`
- **Database Password**: 数据库密码
- **Database SSL Mode**: `disable` | `require` | `prefer`（默认 `disable`）

### 方式 2: JSON 配置（向后兼容，高级用户）

仍然支持原有的 JSON 配置方式，用于复杂场景或高级配置。

## 📝 配置示例

### 示例 1: Azure OpenAI + PGVector（表单方式）

```
Async Mode: ✅ true

LLM Provider: azure_openai
LLM Model: gpt-4o-mini
LLM API Key: sk-xxx...
Azure Endpoint: https://your-resource.openai.azure.com
Azure Deployment: gpt-4o-mini
LLM Temperature: 0.1
LLM Max Tokens: 256

Embedder Provider: azure_openai
Embedder Model: text-embedding-3-small
Embedder API Key: sk-xxx...
Embedder Azure Endpoint: https://your-resource.openai.azure.com
Embedder Azure Deployment: text-embedding-3-small

Vector DB Provider: pgvector
Database Host: localhost
Database Port: 5432
Database Name: mem0_vectors
Database User: postgres
Database Password: your-password
Database SSL Mode: disable
```

### 示例 2: OpenAI + PGVector（表单方式）

```
Async Mode: ✅ true

LLM Provider: openai
LLM Model: gpt-4o-mini
LLM API Key: sk-xxx...
LLM Temperature: 0.1
LLM Max Tokens: 256

Embedder Provider: openai
Embedder Model: text-embedding-3-small
Embedder API Key: sk-xxx...

Vector DB Provider: pgvector
Database Host: localhost
Database Port: 5432
Database Name: mem0_vectors
Database User: postgres
Database Password: your-password
Database SSL Mode: disable
```

### 示例 3: OpenAI + PGVector（表单方式）

```
Async Mode: ✅ true

LLM Provider: openai
LLM Model: gpt-4o-mini
LLM API Key: sk-xxx...
LLM Temperature: 0.1
LLM Max Tokens: 256

Embedder Provider: openai
Embedder Model: text-embedding-3-small
Embedder API Key: sk-xxx...

Vector DB Provider: pgvector
Database Host: localhost
Database Port: 5432
Database Name: mem0_vectors
Database User: postgres
Database Password: your-password
Database SSL Mode: disable
```

## 🔄 优先级机制

如果同时提供了 JSON 和表单字段：
- **JSON 配置优先**（向后兼容）
- 表单字段作为备选方案

## ✅ 优势

1. **简单易用** - 不需要手动输入 JSON
2. **减少错误** - 自动验证和格式化
3. **清晰直观** - 每个字段都有明确的标签和帮助
4. **向后兼容** - 仍然支持 JSON 配置

## 📚 注意事项

- 所有字段都是 `text-input` 类型（最简单）
- 密码字段使用 `secret-input` 类型（隐藏输入）
- **条件字段**：根据选择的 Provider，某些字段可能不需要填写
  - 如果选择 `azure_openai`（推荐），只需要填写 Azure 相关字段
  - 如果选择 `openai`，只需要填写 API Key
  - 如果选择 `anthropic`，不需要填写 API Key
  - 其他不相关的字段可以留空，不会影响配置
- 可选字段可以留空
- 默认值会自动应用


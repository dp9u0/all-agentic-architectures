# CLAUDE.md

## Nebius → OpenAI API 迁移规则

本项目将 notebook 文件从 Nebius AI Studio 迁移到 OpenAI API。以下是标准修改模式：

### 1. 依赖包修改

| 原值               | 新值               |
| ------------------ | ------------------ |
| `langchain-nebius` | `langchain-openai` |

### 2. 导入语句修改

```python
# 原导入
from langchain_nebius import ChatNebius

# 新导入
from langchain_openai import ChatOpenAI
```

### 3. 环境变量修改

| 原环境变量       | 新环境变量                   |
| ---------------- | ---------------------------- |
| `NEBIUS_API_KEY` | `OPENAI_API_KEY`             |
| -                | `OPENAI_API_BASE_URL` (新增) |
| -                | `OPENAI_API_MODEL` (新增)    |

### 4. LLM 初始化修改

```python
# 原代码
llm = ChatNebius(model="meta-llama/Meta-Llama-3.1-8B-Instruct", temperature=0.2)

# 新代码
model = os.environ.get("OPENAI_API_MODEL", "gpt-4o")
base_url = os.environ.get("OPENAI_API_BASE_URL", "https://api.openai.com/v1")
llm = ChatOpenAI(model=model, base_url=base_url, temperature=0.2)
```

### 5. 环境变量检查修改

```python
# 原检查
if not os.environ.get("NEBIUS_API_KEY"):
    print("NEBIUS_API_KEY not found...")

# 新检查
if not os.environ.get("OPENAI_API_KEY"):
    print("OPENAI_API_KEY not found...")
```

### 6. 文档说明修改

在 markdown 文档中：

- "Nebius AI Studio models" → "OpenAI models"
- "Nebius" → "OpenAI"
- LangSmith 项目名称中的 "(Nebius)" → "(OpenAI)"

### 7. .env 文件配置

```shell
OPENAI_API_KEY=your_openai_api_key_here
OPENAI_API_BASE_URL=your_openai_api_base_url_here
OPENAI_API_MODEL=gpt-4o
LANGCHAIN_API_KEY=your_langsmith_api_key_here
```

### 说明

- `ChatOpenAI` 会自动从环境变量读取 `OPENAI_API_KEY`
- `OPENAI_API_BASE_URL` 用于自定义 API 端点（如使用代理或兼容服务）
- `OPENAI_API_MODEL` 允许灵活切换模型，默认为 `gpt-4o`

## 翻译规则

- 仅翻译: source 内容
- 翻译:  markdown 部分( "cell_type": "markdown" )
- 不翻译: 代码部分 ("cell_type": "code" )

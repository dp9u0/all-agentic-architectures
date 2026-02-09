# CLAUDE.md

## OpenAI → Anthropic API 迁移规则

本项目将 notebook 文件从 OpenAI API 迁移到 Anthropic API。以下是标准修改模式：

### 1. 依赖包修改

| 原值               | 新值                  |
| ------------------ | --------------------- |
| `langchain-openai` | `langchain-anthropic` |

### 2. 导入语句修改

```python
# 原导入
from langchain_openai import ChatOpenAI

# 新导入
from langchain_anthropic import ChatAnthropic
```

### 3. 环境变量修改

| 原环境变量            | 新环境变量          |
| --------------------- | ------------------- |
| `OPENAI_API_KEY`      | `ANTHROPIC_API_KEY` |
| `OPENAI_API_BASE_URL` | `BASE_URL`          |
| `OPENAI_API_MODEL`    | `MODEL_NAME`        |

### 4. LLM 初始化修改

```python
# 原代码
model = os.environ.get("OPENAI_API_MODEL", "gpt-4o")
base_url = os.environ.get("OPENAI_API_BASE_URL", "https://api.openai.com/v1")
llm = ChatOpenAI(model=model, base_url=base_url, temperature=0.2)

# 新代码
model = os.environ.get("MODEL_NAME", "claude-opus-4-5-20251101")
base_url = os.environ.get("BASE_URL")
llm = ChatAnthropic(model=model, base_url=base_url, temperature=0.2)
```

### 5. 环境变量检查修改

```python
# 原检查
if not os.environ.get("OPENAI_API_KEY"):
    print("OPENAI_API_KEY not found...")

# 新检查
if not os.environ.get("ANTHROPIC_API_KEY"):
    print("ANTHROPIC_API_KEY not found...")
```

### 6. 文档说明修改

在 markdown 文档中：

- "OpenAI models" → "Anthropic models"
- "OpenAI" → "Anthropic"
- "gpt-4o" → "Claude"
- LangSmith 项目名称移除 API 相关后缀（如 "(OpenAI)"、"(Anthropic)"）

### 7. .env 文件配置

```shell
ANTHROPIC_API_KEY=your_anthropic_api_key_here
MODEL_NAME=claude-opus-4-5-20251101
BASE_URL=https://api.anthropic.com
LANGCHAIN_API_KEY=your_langsmith_api_key_here
```

### 说明

- `ChatAnthropic` 会自动从环境变量读取 `ANTHROPIC_API_KEY`
- `ANTHROPIC_MODEL` 默认为 `claude-3-5-sonnet-20240620`

## 翻译规则

- 仅翻译: source 内容
- 翻译:  markdown 部分( "cell_type": "markdown" )
- 不翻译: 代码部分 ("cell_type": "code" )

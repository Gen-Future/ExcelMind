# ExcelMind 📊

An **Excel Data Intelligence Analysis Assistant** based on LangGraph, supporting natural language queries, multi-turn conversations, streaming output, **ECharts visualization**, and visual thinking process display.

![Python](https://img.shields.io/badge/Python-3.11+-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![FastAPI](https://img.shields.io/badge/FastAPI-Latest-009688)
![LangChain](https://img.shields.io/badge/LangChain-Latest-orange)

## 🎬 Demo Video

[![ExcelMind Demo](https://img.shields.io/badge/📺_Watch-Bilibili_Video-00A1D6?style=for-the-badge&logo=bilibili)](https://www.bilibili.com/video/BV1VKvWBgEF1/)

> The video showcases ExcelMind's core features: natural language queries, intelligent table joining, knowledge base retrieval, etc.

## ✨ Key Features

### 🎯 Core Capabilities
- **Natural Language Queries**: Ask questions in English/Chinese directly, no coding or formulas needed
- **Multi-turn Conversations**: Support contextual follow-up questions (e.g., "How about compared to last month?")
- **Streaming Output**: Real-time display of AI thinking process and responses for smoother interaction
- **Intelligent Tool Calling**: Automatically select appropriate data analysis tools, showing complete reasoning chain

### 🛠️ Rich Data Analysis Tools
| Tool | Function | Features |
|------|----------|----------|
| `filter_data` | Filter + Sort | Support multi-condition AND, sorting, column selection |
| `aggregate_data` | Aggregate Statistics | Support filter before aggregate |
| `group_and_aggregate` | Group Aggregation | Support filter before grouping |
| `search_data` | Keyword Search | Limitable search scope |
| `get_column_stats` | Column Statistics | Support filter before statistics |
| `get_unique_values` | Unique Values | Support filter before retrieval |
| `get_data_preview` | Data Preview | Quick data overview |
| `get_current_time` | Get Time | Handle relative time queries |
| `calculate` | Math Calculation | Batch precise calculation |
| `generate_chart` | **Chart Generation** | ECharts visualization, AI auto-recommends chart types |

### 🔄 Multi-Table Collaboration
- **Multi-Table Management**: Upload and manage multiple Excel tables simultaneously
- **Intelligent Table Joining**: AI automatically analyzes table structures, generates join suggestions via `🤖 Smart Join` feature
- **Flexible Joining**: Support multi-field (composite key) joins, and various join types (Inner/Left/Right/Outer)
- **Context Awareness**: Clearly display current table context during conversations

### 📚 Local Knowledge Base
- **Private Knowledge Storage**: Store business rules, field descriptions, operation guides, and other private knowledge
- **Vector Retrieval**: Based on Chroma vector database, using Embedding models for semantic retrieval
- **Smart Recall**: Automatically retrieve relevant knowledge during conversations, inject into Prompt to improve response quality
- **Web Management**: Visual knowledge entry management in the right panel, supporting CRUD and file upload
- **Persistent Storage**: Knowledge vectors are automatically persisted, no loss after restart

### 📈 ECharts Visualization (NEW)
- **Multiple Chart Types**: Support bar charts, line charts, pie charts, scatter plots, radar charts, funnel charts
- **AI Auto-Recommendation**: Intelligently recommend the most suitable chart type based on data characteristics
- **Interactive Charts**: Based on ECharts 5.5, support hover tooltips, legend toggle, responsive layout
- **Natural Language Trigger**: Simply say "draw a chart" or "visualize sales data" to generate

### 🦺 Modern Web Interface
- **Dual Theme Mode**: Support light/dark theme switching with one click, automatically remember user preferences
- **Sidebar Management**: Clear table list and operation entry
- **Drag & Drop Upload**: Support multi-file drag-and-drop upload with progress indication
- **Real-time Preview**: Instant display of data structure and preview after upload
- **Thinking Visualization**: Display AI's reasoning process (Chain of Thought)
- **Tool Call Display**: Transparent display of each tool call and result (beautified version)
- **Markdown Rendering**: Perfect support for tables, code blocks, and other formats

### 🛡️ Security & Stability
- **Intent Filtering**: Automatically reject casual chats unrelated to Excel data
- **Type Compatibility**: Tool parameters support multiple data types (string, numeric, date)
- **Fuzzy Matching**: Date fields support prefix matching (e.g., "202511" matches "20251104")
- **High Iteration Limit**: Support multi-step reasoning for complex tasks (up to 50 tool calls)

## 🚀 Quick Start

### Requirements
- Python 3.11+
- [uv](https://github.com/astral-sh/uv) (recommended) or pip

### 1. Clone Project

```bash
git clone https://github.com/stark-456/ExcelMind.git
cd ExcelMind
```

### 2. Install Dependencies

```bash
# Using uv (recommended)
uv sync

# Or using pip
pip install -e .
```

### 3. Configuration

Edit `config.yaml` to configure model parameters:

```yaml
model:
  provider: "openai"
  model_name: "gpt-4"  # or other compatible models
  api_key: "your-api-key"
  base_url: "https://api.openai.com/v1"  # optional, custom endpoint
  temperature: 0.1
  max_tokens: 4096

excel:
  max_preview_rows: 20
  default_result_limit: 20
  max_result_limit: 1000

server:
  host: "0.0.0.0"
  port: 8000
```

Or use environment variables:

```bash
export OPENAI_API_KEY="your-api-key"
export OPENAI_BASE_URL="your-api-base-url"
```

### 4. Start Service

```bash
# Web service mode (recommended)
uv run python -m excel_agent.main serve

# CLI mode
uv run python -m excel_agent.main cli --excel your_file.xlsx
```

### 5. Usage

Open browser and visit `http://localhost:8000`:
1. Drag and drop or click to upload Excel files
2. Enter natural language questions in the chat box
3. View AI's thinking process and analysis results

## 📡 API Endpoints

Visit `http://localhost:8000/docs` for complete Swagger documentation after starting the service.

### Main Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Web Interface |
| `/upload` | POST | Upload Excel file |
| `/load` | POST | Load Excel via path |
| `/chat/stream` | POST | Streaming chat (recommended) |
| `/chat` | POST | Non-streaming chat |
| `/status` | GET | Get current status |
| `/reset` | POST | Reset Agent |

### Request Examples

```bash
# Upload Excel
curl -X POST "http://localhost:8000/upload" \
  -F "file=@your_file.xlsx"

# Streaming chat (with history)
curl -X POST "http://localhost:8000/chat/stream" \
  -H "Content-Type: application/json" \
  -d '{
    "message": "How many new mobile users in West District in November 2025?",
    "history": [
      {"role": "user", "content": "What columns are in this table?"},
      {"role": "assistant", "content": "This table contains the following columns: Period, Branch Name, ..."}
    ]
  }'
```

## 🏗️ Project Structure

```
Excel_Agent/
├── config.yaml              # Configuration file
├── pyproject.toml           # Project dependencies
├── README.md
├── knowledge/               # Knowledge base files directory
│   └── *.md                 # Markdown format knowledge files
├── .vector_db/              # Chroma vector database (auto-generated)
└── src/
    └── excel_agent/
        ├── __init__.py
        ├── main.py          # Entry point
        ├── api.py           # FastAPI endpoints
        ├── config.py        # Configuration management
        ├── excel_loader.py  # Excel loader
        ├── graph.py         # LangGraph workflow
        ├── knowledge_base.py # Knowledge base management
        ├── prompts.py       # Prompt templates
        ├── stream.py        # Streaming chat core
        ├── tools.py         # Data analysis tools
        └── frontend/
            └── index.html   # Web interface
```

## 🐳 Docker Deployment

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY . .
RUN pip install uv && uv sync
EXPOSE 8000
CMD ["uv", "run", "python", "-m", "excel_agent.main", "serve"]
```

```bash
docker build -t excel-agent .
docker run -p 8000:8000 -e OPENAI_API_KEY=your-key excel-agent
```

## 🔧 Development

```bash
# Install dev dependencies
uv sync --dev

# Run tests (if any)
uv run pytest

# Code formatting
uv run ruff format .
```

## 📝 Usage Examples

```
User: How many rows of data are in this table?
Assistant: This table has 15,234 rows of data.

User: Total new mobile users by branch
Assistant: [Calls group_and_aggregate tool]
      New mobile users statistics by branch:
      | Branch | New Mobile Users |
      |--------|------------------|
      | East   | 45,678           |
      | West   | 38,901           |
      | ...    | ...              |

User: Details for West District?
Assistant: [Understands context, calls filter_data]
      Detailed data for West District branch:...

User: Show proportion by branch using a pie chart
Assistant: [Calls generate_chart tool]
      📊 Pie chart generated, 8 data points in total.
      [Interactive ECharts pie chart displayed]
```

## 🤝 Contributing

Issues and Pull Requests are welcome!

## 📄 License

[MIT License](LICENSE)

---

**Made with ❤️ using LangGraph, FastAPI, and OpenAI**

## 💬 Community

![QQ Group Card](docs/card.png)

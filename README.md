# 🔬 LangChain Multi-Agent Research System

> An AI-powered multi-agent research pipeline that searches the web, extracts relevant information, generates structured reports, and evaluates the final output through a dedicated critic.

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)
![LangChain](https://img.shields.io/badge/LangChain-Agentic%20AI-green)
![Streamlit](https://img.shields.io/badge/Streamlit-UI-red?logo=streamlit)
![Tavily](https://img.shields.io/badge/Tavily-Web%20Search-orange)
![License](https://img.shields.io/badge/License-Apache%202.0-lightgrey)

---

## 🧠 Overview

This project demonstrates how multiple specialized AI components can collaborate to automate an end-to-end research workflow.

Instead of relying on a single LLM prompt, the system separates the research process into specialized stages:

**Search → Read → Write → Critique**

Given a research topic, the system:

1. Searches the web for relevant information.
2. Selects and extracts content from useful sources.
3. Generates a structured research report.
4. Evaluates the generated report using a dedicated critic chain.
5. Presents the complete workflow through an interactive Streamlit interface.

The project is designed as a practical demonstration of **Agentic AI, tool calling, web research, prompt chaining, and multi-stage LLM workflows**.

---

## 🏗️ System Architecture

```text
                         ┌──────────────────────┐
                         │      User Topic      │
                         └──────────┬───────────┘
                                    │
                                    ▼
                    ┌─────────────────────────────┐
                    │       Search Agent          │
                    │                             │
                    │  Tavily Web Search Tool    │
                    └──────────────┬──────────────┘
                                   │
                                   ▼
                    ┌─────────────────────────────┐
                    │       Reader Agent          │
                    │                             │
                    │  URL Selection + Scraping   │
                    └──────────────┬──────────────┘
                                   │
                                   ▼
                    ┌─────────────────────────────┐
                    │       Writer Chain          │
                    │                             │
                    │  Structured Research Report │
                    └──────────────┬──────────────┘
                                   │
                                   ▼
                    ┌─────────────────────────────┐
                    │       Critic Chain          │
                    │                             │
                    │  Score + Strengths +        │
                    │  Areas for Improvement      │
                    └──────────────┬──────────────┘
                                   │
                                   ▼
                         ┌──────────────────┐
                         │   Final Output   │
                         └──────────────────┘
```

---

## ✨ Key Features

### 🤖 Multi-Agent Research Workflow

The system divides the research process into specialized components:

| Component       | Responsibility                             |
| --------------- | ------------------------------------------ |
| 🔎 Search Agent | Finds recent and relevant information      |
| 📖 Reader Agent | Extracts deeper content from selected URLs |
| ✍️ Writer Chain | Generates a structured research report     |
| 🧠 Critic Chain | Reviews and scores the generated report    |

This separation makes each stage easier to understand, maintain, and extend.

---

### 🌐 Web Search with Tavily

The Search Agent uses the Tavily API to retrieve:

* Source titles
* URLs
* Search snippets
* Recent web information

The search tool returns up to five results for each research query.

```python
results = tavily.search(
    query=query,
    max_results=5
)
```

---

### 🕷️ Robust Web Content Extraction

The Reader Agent uses a multi-level extraction strategy.

```text
URL
 │
 ▼
Trafilatura
 │
 ├── Successful → Return extracted article
 │
 ▼
Readability
 │
 ├── Successful → Return cleaned content
 │
 ▼
BeautifulSoup
 │
 └── Fallback extraction
```

This approach improves robustness when different websites have different HTML structures.

The extraction pipeline removes unnecessary elements such as:

* scripts
* styles
* navigation
* headers
* footers
* sidebars
* forms

---

### ✍️ Structured Report Generation

The Writer Chain uses a dedicated research-writing prompt.

Every report follows a predefined structure:

```text
Introduction

Key Findings
- Finding 1
- Finding 2
- Finding 3+

Conclusion

Sources
- URL 1
- URL 2
- ...
```

This makes the generated output more consistent and easier to read.

---

### 🧪 AI-Powered Criticism

The generated report is passed to a separate Critic Chain.

The critic evaluates the report using:

```text
Score: X/10

Strengths:
- ...
- ...

Areas to Improve:
- ...
- ...

One line verdict:
...
```

This introduces a basic **generation → evaluation** pattern instead of treating the first generated answer as final.

---

## 🔄 End-to-End Workflow

```text
User enters research topic
            │
            ▼
      Search Agent
            │
            ▼
     Tavily Web Search
            │
            ▼
      Search Results
            │
            ▼
       Reader Agent
            │
            ▼
     URL Content Extraction
            │
            ▼
     Research Combination
            │
            ▼
       Writer Chain
            │
            ▼
      Research Report
            │
            ▼
       Critic Chain
            │
            ▼
   Evaluation & Feedback
```

The main orchestration logic is implemented in `run_research_pipeline()`.

---

## 🖥️ Interactive Streamlit Interface

The project includes a custom Streamlit interface designed specifically for the research workflow.

The interface provides:

* Research topic input
* Pipeline execution button
* Visual pipeline progress
* Search results
* Scraped content
* Generated report
* Critic feedback

The UI uses a custom dark interface with dedicated cards for each research stage.

---

## 🛠️ Tech Stack

### AI / LLM

* Python
* LangChain
* LangChain Agents
* OpenAI `gpt-4o-mini`

### Search

* Tavily Search API

### Web Extraction

* Requests
* Trafilatura
* Readability
* BeautifulSoup
* lxml

### Interface

* Streamlit

### Configuration

* python-dotenv

### Development

* Rich

---

## 📁 Project Structure

```text
LangChain-Multi-Agent-Research-System/
│
├── app.py
├── main.py
├── requirements.txt
├── README.md
├── LICENSE
├── demo.excalidraw
│
└── src/
    │
    ├── __init__.py
    │
    ├── agents/
    │   ├── __init__.py
    │   └── agents.py
    │
    ├── pipelines/
    │   ├── __init__.py
    │   └── pipeline.py
    │
    └── tools/
        ├── __init__.py
        └── tools.py
```

### `src/agents/`

Contains the specialized AI components:

* Search Agent
* Reader Agent
* Writer Chain
* Critic Chain

### `src/tools/`

Contains external tools used by the agents:

* `web_search`
* `scrape_url`

### `src/pipelines/`

Contains the main research orchestration logic.

### `app.py`

Provides the interactive Streamlit application.

---

## ⚙️ Installation

### 1. Clone the repository

```bash
git clone https://github.com/1Huseyn4/LangChain-Multi-Agent-Research-System.git

cd LangChain-Multi-Agent-Research-System
```

> The repository currently contains the project inside the `LangChain-Multi-Agent-Research-System/` directory, so adjust the working directory if necessary.

---

### 2. Create a virtual environment

#### Windows

```bash
python -m venv .venv

.venv\Scripts\activate
```

#### macOS / Linux

```bash
python -m venv .venv

source .venv/bin/activate
```

---

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

---

## 🔑 Environment Variables

Create a `.env` file in the project directory:

```env
OPENAI_API_KEY=your_openai_api_key
TAVILY_API_KEY=your_tavily_api_key
```

The project currently uses OpenAI's `gpt-4o-mini` as its LLM and Tavily for web search.

---

## 🚀 Running the Application

### Streamlit Interface

From the directory containing `app.py`:

```bash
streamlit run app.py
```

Then open the local Streamlit URL shown in the terminal.

---

### Python Pipeline

The research pipeline can also be executed directly from Python:

```python
from src.pipelines.pipeline import run_research_pipeline

result = run_research_pipeline(
    "Future of AI Agents"
)

print(result["report"])
print(result["feedback"])
```

---

## 📊 Example Output

For a research topic such as:

```text
Future of AI Agents in Software Engineering
```

the system performs:

```text
🔎 Searching the web...

        ↓

📖 Extracting relevant source content...

        ↓

✍️ Generating research report...

        ↓

🧠 Critiquing generated report...

        ↓

📄 Final Report + Evaluation
```

---

## 🎯 What This Project Demonstrates

This project focuses on several important concepts in modern AI engineering.

### Agentic AI

Building AI components that can use external tools to perform tasks rather than relying only on static LLM responses.

### Tool Calling

The Search and Reader agents interact with external tools:

```text
Search Agent → web_search()
Reader Agent → scrape_url()
```

### Prompt Chaining

The project demonstrates sequential LLM processing:

```text
Research
   ↓
Generation
   ↓
Evaluation
```

### Multi-Agent Architecture

Different responsibilities are separated between specialized components rather than placing everything into one large prompt.

### Web Research Automation

The system combines search APIs and web content extraction to automatically gather external information.

### LLM Evaluation

The Critic Chain provides a separate evaluation stage for the generated report.

---

## 🧩 Design Principles

### Separation of Responsibilities

Each component has a focused responsibility.

```text
Search  → Find information
Reader  → Understand sources
Writer  → Generate report
Critic  → Evaluate report
```

This makes the architecture easier to extend.

### Modular Tools

The search and scraping functionality is implemented as independent tools, making it possible to replace or add tools later.

### Pipeline-Based Execution

The research process follows a predictable sequence, making the workflow easier to debug and reason about.

---

## 🔮 Future Improvements

Potential extensions include:

* [ ] Add a Supervisor Agent
* [ ] Add iterative Writer ↔ Critic feedback loops
* [ ] Allow the critic to trigger report revisions
* [ ] Add multiple independent research agents
* [ ] Add parallel research execution
* [ ] Add source credibility verification
* [ ] Add citation validation
* [ ] Add structured Pydantic outputs
* [ ] Add persistent research memory
* [ ] Add LangGraph orchestration
* [ ] Add local LLM support through Ollama
* [ ] Add research result export to Markdown/PDF
* [ ] Add automated evaluation metrics
* [ ] Add LangSmith tracing and observability

---

## ⚠️ Current Limitations

This project is intentionally a focused demonstration of a multi-stage research architecture.

Current limitations include:

* The workflow is sequential rather than parallel.
* The critic provides feedback but does not automatically send the report back to the writer.
* Research currently relies on a single search provider.
* The Reader Agent processes selected source content rather than building a persistent knowledge base.
* There is no persistent agent memory.
* The current LLM configuration uses OpenAI.

These limitations also provide clear directions for future development.

---

## 📚 Learning Outcomes

By building this project, the following concepts are practiced:

* LangChain Agents
* Tool Calling
* Prompt Templates
* LLM Chains
* Multi-Agent Architecture
* Web Search
* Web Scraping
* Information Extraction
* Research Automation
* LLM-based Evaluation
* Streamlit Application Development
* Modular Python Architecture

---

## 🚀 Roadmap

```text
Current
  │
  ├── Search Agent
  ├── Reader Agent
  ├── Writer Chain
  └── Critic Chain
        │
        ▼
Next
  │
  ├── Supervisor Agent
  ├── Iterative Critic Loop
  ├── Parallel Research
  ├── Structured Outputs
  ├── LangGraph
  └── Persistent Memory
        │
        ▼
Advanced
  │
  ├── RAG Integration
  ├── Source Verification
  ├── Citation Validation
  ├── Multi-Agent Planning
  └── Production Observability
```

---

## 💼 Why This Project Matters

A major goal of modern AI engineering is moving beyond simple:

```text
User → LLM → Answer
```

toward systems where AI models can:

```text
User
  ↓
Plan / Search
  ↓
Use Tools
  ↓
Collect Information
  ↓
Generate
  ↓
Evaluate
  ↓
Improve
```

This project demonstrates the foundations of that transition by combining **LLMs, tools, specialized agents, external information retrieval, structured prompting, and automated evaluation** in one workflow.

---

## 👨‍💻 Author

**Huseyn Mammadov**

AI / Machine Learning Engineer in progress

Interested in:

* Artificial Intelligence
* Machine Learning
* LLMs
* RAG Systems
* Agentic AI
* Multi-Agent Systems
* LangChain / LangGraph

---

## 📄 License

This project is licensed under the Apache License 2.0.

See [`LICENSE`](./LICENSE) for details.

---

⭐ If you find this project useful, consider giving it a star.

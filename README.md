# Multi-Analyst Research Assistant

An advanced AI-powered research system that generates multiple specialized analysts to conduct parallel interviews with AI experts, then synthesizes their findings into a comprehensive report.

## What It Does

This sophisticated research tool:
- **Generates AI Analysts**: Creates specialized analysts based on your research topic
- **Human-in-the-Loop**: Allows you to review and refine the analyst team before research begins
- **Parallel Interviews**: Each analyst conducts in-depth interviews with AI experts simultaneously
- **Multi-Source Research**: Uses both web search (Tavily) and Wikipedia for comprehensive information gathering
- **Report Generation**: Synthesizes all findings into a professional report with introduction, insights, and conclusion
- **Proper Citations**: All claims are backed by numbered citations and source links

## Architecture

The system uses a **multi-agent architecture** with two main graph structures:

1. **Analyst Generation Graph** (with human feedback loop)
   - Creates specialized analysts for different aspects of your topic
   - Allows human review and modification of the analyst team
   
2. **Research Graph** (with parallel sub-graphs)
   - Each analyst runs an independent interview in parallel (map step)
   - Interviews use both web search and Wikipedia for information
   - Results are synthesized into a final report (reduce step)

## Key Features

- **🤖 Multiple AI Analysts**: Automatically generates 2-3 specialized analysts for your topic
- **👥 Human Review**: Review and provide feedback on analysts before research begins
- **⚡ Parallel Processing**: All interviews run simultaneously using LangGraph's Send API
- **🔍 Dual Search**: Combines Tavily web search and Wikipedia for comprehensive coverage
- **📝 Professional Reports**: Generates structured reports with intro, insights, conclusion, and sources
- **🎯 Focused Research**: Each analyst explores a specific sub-topic in depth

## Requirements

- Python 3.8+
- Google Colab (recommended) or local Jupyter environment
- Required API keys:
  - OpenAI API key (for GPT-4)
  - LangSmith API key (for tracing)
  - Tavily API key (for web search)

## Installation

### Option 1: Google Colab (Recommended)

1. Open the notebook in Google Colab
2. The notebook will automatically install required packages:
   ```python
   %pip install langgraph langchain_openai langchain_community langchain_core tavily-python wikipedia langchain_tavily
   ```

### Option 2: Local Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/research-assistant.git
cd research-assistant

# Install dependencies
pip install langgraph langchain_openai langchain_community langchain_core tavily-python wikipedia langchain_tavily
```

## Setup

1. **Get API Keys**:
   - OpenAI: [platform.openai.com](https://platform.openai.com)
   - LangSmith: [smith.langchain.com](https://smith.langchain.com)
   - Tavily: [tavily.com](https://tavily.com)

2. **Configure the Notebook**:
   The notebook will prompt you for:
   - `OPENAI_API_KEY`
   - `LANGSMITH_API_KEY`
   - `TAVILY_API_KEY`

3. **Set Research Parameters**:
   ```python
   max_analysts = 3  # Number of analysts (2-3 recommended)
   topic = "Your research topic here"
   ```

## How It Works

### Step 1: Generate Analysts
```python
# System generates specialized analysts
analysts = create_analysts(topic, max_analysts)
# Example output:
# - Technology Adoption Specialist
# - Data Security Analyst
# - Sustainability Consultant
```

### Step 2: Human Review
```python
# Review analysts and provide feedback
human_feedback = "Add someone from a startup for entrepreneur perspective"
# System regenerates analysts incorporating your feedback
```

### Step 3: Parallel Interviews
```python
# Each analyst conducts 2-turn interview with expert
# Uses web search and Wikipedia for information
# Runs all interviews in parallel
```

### Step 4: Report Generation
```python
# System synthesizes all findings into:
# - Introduction
# - Insights (main content)
# - Conclusion
# - Sources
```

## Example Usage

```python
# Define research topic
topic = "The benefits of adopting LangGraph as an agent framework"
max_analysts = 3
thread = {"configurable": {"thread_id": "1"}}

# Run until first interruption (analyst review)
for event in graph.stream({"topic": topic, "max_analysts": max_analysts}, thread):
    # Review generated analysts
    analysts = event.get('analysts', '')
    
# Provide feedback (optional)
graph.update_state(thread, {
    "human_analyst_feedback": "Add a startup CEO perspective"
}, as_node="human_feedback")

# Continue research
for event in graph.stream(None, thread):
    pass

# Get final report
final_state = graph.get_state(thread)
report = final_state.values.get('final_report')
```

## Output Format

The final report includes:

```markdown
# [Title]

## Introduction
[Context-setting introduction]

---

## Insights
[Synthesized findings from all analyst interviews]

---

## Conclusion
[Summary and key takeaways]

## Sources

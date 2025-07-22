# 🦎Chameleon: AI-Powered Headline Generator

Chameleon is a Python-based, AI-powered agent that automatically generates multiple headline variations for any given news article URL. It's designed to help content creators, marketers, and journalists quickly craft compelling headlines for their articles. By providing a URL, the agent summarizes the content and then produces factual, creative, and question-based headlines, along with identifying the target audience.

## 📝About The Project

This project leverages the power of large language models (LLMs) through the LangChain framework to create a sophisticated agent that can understand and process web content. The agent is built using a "ReAct" (Reasoning and Acting) framework, allowing it to dynamically decide which tools to use to accomplish its goal.

The key features of Chameleon include:

* **Article Summarization:** It can fetch and summarize the content of any public news article.
* **Multi-Style Headline Generation:** It creates a diverse set of headlines to appeal to different reader segments.
* **Target Audience Identification:** It analyzes the content to suggest the most relevant audience.
* **Interactive Interface:** It provides a simple command-line interface for easy interaction.

## 🚀Getting Started

Follow these instructions to get a local copy up and running.

### Prerequisites

You need to have Python 3.7 or later installed on your system. You will also need to install the required Python packages.

### Installation

1.  **Clone the repository (or download the script):**
    ```bash
    git clone https://github.com/Bhavanam-Gireesh-Reddy/Chameleon-News-Headline-Generator-.git
    ```

2.  **Create a virtual environment (recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows use `venv\Scripts\activate`
    ```

3.  **Install the required libraries:**
    ```bash
    pip install langchain langchain-groq langchain-community pydantic python-dotenv beautifulsoup4
    ```

4.  **Set up your environment variables:**
    You will need an API key from [Groq](https://console.groq.com/keys) to use the LLaMA 3 model.
    * Create a file named `.env` in the same directory as your script.
    * Add your Groq API key to the `.env` file like this:
        ```
        GROQ_API_KEY="YOUR_API_KEY_HERE"
        ```

## ⚙️How It Works

Chameleon operates as an intelligent agent that can use a set of predefined tools to achieve its objectives. Here’s a step-by-step breakdown of its internal workflow:

1.  **Input:** The user provides a URL to a news article.

2.  **Tool Selection:** The agent, powered by a ReAct framework, determines that it first needs to understand the article's content. It selects the `get_article_summary` tool for this purpose.

3.  **Article Summarization (`get_article_summary` tool):**
    * The tool uses `WebBaseLoader` from `langchain_community` to load the content from the provided URL.
    * The loaded text is then split into smaller, manageable chunks using `RecursiveCharacterTextSplitter`.
    * A summarization chain (`load_summarize_chain` with a map-reduce strategy) processes these chunks to create a concise summary of the article.

4.  **Reasoning and Next Step:** With the summary in hand, the agent reasons that it now has enough information to generate headlines. It then selects the `generate_headlines` tool.

5.  **Headline Generation (`generate_headlines` tool):**
    * This tool is designed to produce structured output. It uses a Pydantic model (`HeadlineSet`) to define the exact format it needs: a factual headline, a creative one, a question-based one, and the target audience.
    * It uses a `PydanticOutputParser` to ensure the LLM's response conforms to this structure.
    * A prompt template (`ChatPromptTemplate`) instructs the LLM to act as an expert copywriter and generate the headlines based on the provided summary and formatting instructions.
    * The tool invokes the LLM with the summary and returns the structured set of headlines.

6.  **Final Output:** The agent receives the headlines from the tool and presents the final, formatted answer to the user.

## ▶️Usage

Once you have completed the installation and setup, you can run the agent from your terminal.

1.  Make sure your virtual environment is activated.
2.  Run the Python script.
3.  When prompted, enter the URL of the news article you want to process.

**Example Interaction:**

```
🤖 Agent is ready. Ask me to generate headlines for a URL.
   Example: Get headlines for [https://www.your-news-article-url.com](https://www.your-news-article-url.com)
   Type 'exit' to quit.

You: [https://edition.cnn.com/2024/05/21/politics/trump-netanyahu-israel-gaza-iran/index.html](https://edition.cnn.com/2024/05/21/politics/trump-netanyahu-israel-gaza-iran/index.html)

> Entering new AgentExecutor chain...
➡️ [Tool Running: get_article_summary] Loading content from ...
➡️ [Tool Running: get_article_summary] Splitting and summarizing...
➡️ [Tool Running: get_article_summary] Summary complete.
➡️ [Tool Running: generate_headlines] Generating headlines...
➡️ [Tool Running: generate_headlines] Headlines generated.
> Finished chain.

✅ Agent's Final Answer:
- Factual Headline: US-Israel Relations Strained Over Gaza and Iran Policies
- Creative Headline: White House Walks a Tightrope Between Allies and Adversaries
- Question Headline: Can the US and Israel Find Common Ground on Middle East Strategy?
--------------------------------------------------
```

To stop the agent, simply type `exit` and press Enter.

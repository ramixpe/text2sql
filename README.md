Vanna-LGX: A Local-First, Agentic Text-to-SQL System
Vanna-LGX is a project to build a sophisticated, local-first Text-to-SQL agent using a graph-based architecture. It leverages local LLMs via Ollama and a local vector store to provide a private, powerful, and extensible data analysis tool.

The project is developed in stages, with each stage adding a new layer of intelligence and robustness.

📍 Current Stage: S2 - Few-shot Example Aware Agent

The agent uses Retrieval-Augmented Generation (RAG) to dynamically fetch relevant table schemas (DDL).

It also retrieves curated, human-approved Question -> SQL examples to learn complex query patterns at runtime.

This "few-shot" approach dramatically improves the quality and complexity of the generated SQL.

Architecture (Stage S2)

The agent operates as a state machine orchestrated by LangGraph.

Code snippet
graph TD
    A[User Question] --> B(retrieve_context);
    B -- DDL & SQL Examples --> C(synthesize_sql);
    C -- Generated SQL --> D(execute_sql);
    D -- Query Result --> E(summarize_result);
    E --> F[Final Answer];
Tech Stack

Orchestration: LangGraph

LLM Backend: Ollama (e.g., gpt-oss, llama3)

Embedding Models: Ollama (e.g., mxbai-embed-large, nomic-embed-text)

Vector Database: ChromaDB (local persistence)

Database: SQLite

Setup and Installation

1. Clone the Repository

Bash
git clone https://github.com/ramixpe/text2sql.git
cd text2sql
2. Set up Environment
A Conda or Python virtual environment is highly recommended.

Bash
# Using Conda
conda create -n vanna_lgx python=3.11
conda activate vanna_lgx
3. Install Dependencies
Ensure you have a requirements.txt file with all necessary packages.

langchain
langgraph
langchain-ollama
pandas
pydantic
chromadb
tiktoken
Then install them:

Bash
pip install -r requirements.txt
4. Set up Ollama
Ensure the Ollama application is installed and running. Pull the models required for this project:

Bash
# Model for SQL Synthesis and Summarization
ollama pull gpt-oss

# Model for creating text embeddings
ollama pull mxbai-embed-large
(Note: You can change the models used in vanna_lgx/config.py)

5. Place Your Database
Place your SQLite database file (e.g., unoc_19_jan.db) inside the data/ directory.

Data Ingestion

Before running the agent, you must populate the local knowledge base (ChromaDB).

1. Prepare Knowledge Files

DDL: This is extracted automatically from your database in the data/ directory.

SQL Examples: Add your high-quality Question -> SQL pairs to knowledge/sql_examples/examples.json. The file should be a JSON array of objects, where each object has a "question" and "sql" key.

2. Run Ingestion Scripts
Execute these commands from the project root directory. These scripts create a chroma/ directory which is your local vector store.

Bash
# Ingest the database schema into the 'ddl' collection
python -m scripts.ingest_ddl

# Ingest the SQL examples into the 'sql_examples' collection
python -m scripts.ingest_sql_examples
Running the Agent

Once setup and ingestion are complete, you can start the interactive agent.

Bash
python -m vanna_lgx.main
Project Structure

.
├── knowledge/
│   └── sql_examples/
│       └── examples.json
├── scripts/
│   ├── ingest_ddl.py
│   └── ingest_sql_examples.py
├── vanna_lgx/
│   ├── core/
│   │   ├── graph.py
│   │   ├── nodes.py
│   │   └── state.py
│   ├── utils/
│   ├── config.py
│   └── main.py
├── data/
│   └── unoc_19_jan.db
├── .gitignore
└── README.md

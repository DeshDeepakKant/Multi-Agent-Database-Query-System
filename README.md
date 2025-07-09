# Multi-Agent Database Query System

[![Python 3.9+](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![LangChain](https://img.shields.io/badge/LangChain-b03a2e)](https://www.langchain.com/)
[![LangGraph](https://img.shields.io/badge/LangGraph-orange)](https://github.com/langchain-ai/langgraph)
[![Streamlit](https://img.shields.io/badge/Streamlit-ff4b4b)](https://streamlit.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Overview

Multi-Agent Database Query System is an advanced, multi-agent Retrieval-Augmented Generation (RAG) system designed for enterprise applications. Built with flexibility and scalability in mind, it enables users to pose complex questions in natural language and receive comprehensive answers synthesized from diverse database systems: **SQLite, MongoDB, MeiliSearch, and Neo4j**.

## Architecture Overview

## Project Structure

The project is organized into several key directories:

- **`agents/`**: Contains the core logic for each agent (Router, Refiner, etc.).
  - `router.py`
  - `...`
- **`database/`**: Scripts for creating and populating the databases.  
  - `populate_db.py`
- **`graph/`**: Defines the structure and flow of the agentic graph.
  - `builder.py`
- **`tools/`**: Contains tools for database-specific query generation.
  - `db_tools.py`
- **`ui/`**: The Streamlit user interface code.
  - `app_ui.py`
- **`.env.example`**: Example environment variables file.
- **`app.py`**: Main entry point for the command-line application.
- **`requirements.txt`**: Project dependencies.

The system leverages `LangGraph` to create a stateful, cyclical graph of agents that work collaboratively to answer queries.

1.  **Router Agent**: First, a router agent analyzes the user's question to determine the most appropriate database (or databases) to query.
2.  **Specialized Agents**: The query is then passed to one or more specialized agents, each an expert in generating queries for a specific database (SQL, Cypher, etc.).
3.  **Query Execution**: The generated query is executed against the corresponding database.
4.  **Answer Synthesis**: The results are synthesized into a coherent, human-readable answer.
5.  **Self-Correction Loop**: If a query fails or returns no results, a **Refiner Agent** attempts to rewrite the query and retry the process, creating a self-correcting loop.
6.  **Clarification Questions**: If the initial question is ambiguous, the system can ask the user for clarification before proceeding.

```mermaid
graph TD
    subgraph "User Interaction"
        A[User Query]
    end

    subgraph "Core Agent System<br>(LangGraph)"
        B(Router Agent)
        C(Refiner Agent)

        subgraph "Specialized Agents"
            D1[SQLite Agent]
            D2[MongoDB Agent]
            D3[MeiliSearch Agent]
            D4[Neo4j Agent]
        end

        subgraph "Data Sources"
            DB1[(SQLite DB)]
            DB2[(MongoDB)]
            DB3[(MeiliSearch)]
            DB4[(Neo4j DB)]
        end
    end

    subgraph "Final Output"
        E[Synthesized Answer]
    end

    A --> B
    B -- Chooses DB --> D1
    B -- Chooses DB --> D2
    B -- Chooses DB --> D3
    B -- Chooses DB --> D4

    D1 --> DB1
    D2 --> DB2
    D3 --> DB3
    D4 --> DB4

    DB1 -- Success --> E
    DB2 -- Success --> E
    DB3 -- Success --> E
    DB4 -- Success --> E

    D1 -- Failure --> C
    D2 -- Failure --> C
    D3 -- Failure --> C
    D4 -- Failure --> C

    C -- "Rewrites Query" --> B
```

## Key Features

* **Multi-Database Support**: Simultaneously query relational, document, search, and graph databases.
* **Intelligent Routing**: Automatically selects the best data source for a given question.
* **Self-Correcting Agents**: Agents can refine and correct their own queries upon failure.
* **Interactive UI**: A user-friendly interface built with Streamlit to visualize the process.
* **Modular Design**: Easily extendable to include new agents and data sources.

## Technologies Used

* **Orchestration**: LangChain & LangGraph
* **LLMs**: OpenAI
* **Databases**:
    * Relational: SQLite
    * Document: MongoDB
    * Search: MeiliSearch
    * Graph: Neo4j
* **UI**: Streamlit
* **Environment Management**: python-dotenv

## Getting Started

### Prerequisites

* Python 3.9 or higher
* An OpenAI API Key | We recommend TogetherAPIs

### Installation

Clone the repository and install the required dependencies.

```bash
git clone https://github.com/DeshDeepakKant/Multi-Agent-Database-Query-System
cd Multi-Agent-Database-Query-System
pip install -r requirements.txt
```

### Environment Setup

Create a `.env` file in the root directory by copying the example file:

```bash
cp .env.example .env
```

Now, open the `.env` file and add your OpenAI API key:

```
OPENAI_API_KEY="sk-..."
```

### Populate Databases

Run the `populate_db.py` script to create and fill the databases with sample data.

```bash
python -m database.populate_db
```

### Usage

You can interact with the system in two ways:

**A) Interactive Streamlit UI (Recommended)**

Launch the Streamlit application:

```bash
streamlit run ui/app_ui.py
```

Open your browser to the local URL provided to start asking questions.

**B) Command-Line Interface (CLI)**

Run the main application script:

```bash
python app.py
```

You will be prompted to enter your questions in the terminal.



## Contributing

Contributions are welcome! If you have ideas for new features or improvements, please open an issue or submit a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contact

**Desh Deepak Kant**  
Email: [deshdeepakkant@gmail.com](mailto:deshdeepakkant@gmail.com)  
LinkedIn: [https://www.linkedin.com/in/deshdeepakkant/](https://www.linkedin.com/in/deshdeepakkant/)  
GitHub: [https://github.com/DeshDeepakKant](https://github.com/DeshDeepakKant)

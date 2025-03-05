# Competitor Search API

This project provides an API to fetch the top 10 national competitors for a given company using LLMs and hybrid search techniques.
The response will have the competitor's name, reference link, llm confidence score based on the criteria mentioned.
Crtieria: multiple occurrence and position in search results.

## Project Structure
```
.
├── Dockerfile
├── README.md
├── data
│   ├── cache.json
│   ├── competitor_data.pkl
│   ├── competitor_index.ann
│   └── samples.json
├── docs
│   ├── architecture.md
│   └── enhancements.md
├── logs
│   └── app_2025-03-04.log
├── pyproject.toml
├── requirements.txt
├── src
│   ├── __init__.py
│   ├── config.py
│   ├── generation.py
│   ├── main.py
│   ├── pipeline.py
│   ├── search.py
│   ├── utils.py
│   └── validation.py
├── tests
│   └── __init__.py
└── uvicorn_start.sh
```

## Installation
1. Navigate to the project folder:
    ```sh
    cd <PROJECT_FOLDER>
    ```
2. Install dependencies:
    ```sh
    pip install -r requirements.txt
    ```
3. Set up environment variables:
    - Create a `.env` file and add the required API keys:
      ```env
      GROQ_API_KEY=<your_api_key>
      SERPER_API_KEY=<your_api_key>
      SERPAPI_API_KEY=<your_api_key>
      TOKENIZERS_PARALLELISM=false
      ```
    - Get API keys:
      - [Serper API Key](https://serper.dev/)
      - [Groq API Key](https://groq.com/)

## Running the API
Start the FastAPI server:
```sh
uvicorn src.main:app --host 0.0.0.0 --port 8000
```

## API Endpoints
### `POST /get_competitors/`
Fetch the top 10 competitors for a given company.
#### Request Body:
```json
{
    "company_name": "Example Company",
    "country_code": "in"
}
```
#### Response:
```json
[
    {
        "competitor_name": "Competitor 1",
        "source": "https://example.com",
        "confidence_score": 0.95 
    },
    ...
]
```

## Components
### `config.py`
Handles environment variable loading and stores model parameters.
### `generation.py`
Uses an LLM to generate competitor lists based on search results.
### `main.py`
FastAPI application that handles requests and caching.
### `pipeline.py`
Builds and runs the LangGraph-based workflow for retrieving competitors.
### `search.py`
Performs hybrid search and maintains an Annoy index for fast retrieval.
### `utils.py`
Utility functions for parsing and normalizing data.
### `validation.py`
Handles response validation and JSON extraction.

## Caching Mechanism
- The API caches results for 5 minutes using `data/cache.json` to reduce redundant API calls.

## Running Tests
To run tests: [To be updated]
```sh
pytest tests/
```

## Deployment
To deploy using Docker:
```sh
docker build -t competitor-search-api .
docker run -p 8000:8000 competitor-search-api
```

## Enhancements (Event-Driven Architecture) - Below are the thought process on this project.
- **Event-Driven Architecture:** Kafka for async processing
- **FAISS + ElasticSearch:** Hybrid retrieval (semantic + text-based search)
- **Redis Caching + MongoDB:** Faster lookup and persistence
- **Multi-Provider Web Search:** Avoid single dependency (Serper API, Google, Bing)
- **Model-Based Competitor List Generation:** LLM / (heuristic + custom model based scoring)
- **Scoring Mechanism:** Better scoring and ranking strategies based on additional features.
- **Better llm deployment and prompt management** - Prompt management for different model providers and ratelimit, deployment strategies.
- **Code refactoring inside src with better practices** 


<<<<<<< HEAD
# IoT Data Intelligence Tool

Natural language analytics for IoT data — runs 100% locally.

```
Stack: React + Vite · FastAPI · Pandas · Ollama · Sentence Transformers + FAISS
```

---

## Architecture

```
User → React UI → FastAPI → RAG (FAISS + SentenceTransformers)
                          → Ollama (local LLM) → JSON plan
                          → Pandas executor → result
                          → React (table / chart)
```

---

## Setup

### 1. Install Ollama + pull a model

```bash
# Install: https://ollama.com
ollama pull qwen2.5:7b      # or: llama3.2, mistral, phi3
ollama serve                 # starts on localhost:11434
```

> If you use a different model, update `MODEL` in `backend/llm.py`.

### 2. Backend

```bash
cd backend
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

### 3. Frontend

```bash
cd frontend
npm install
npm run dev      # → http://localhost:5173
```

---

## Usage

1. Open **http://localhost:5173**
2. Upload a CSV or Excel file
3. Ask questions in plain English:
   - *"Show average temperature for each device"*
   - *"Which device consumed the most energy?"*
   - *"Devices with humidity above 70%"*
   - *"Show energy consumption over time"*

Results appear as a bar / line / pie / scatter chart + table.

---

## Sample CSV format

```csv
timestamp,device_id,temperature,humidity,energy_consumption,location
2024-01-01 00:00,dev_A,22.5,60,150,floor_1
2024-01-01 01:00,dev_B,23.1,65,200,floor_2
...
```

---

## Files

```
iot-llm-tool/
├── backend/
│   ├── main.py          FastAPI app + safe Pandas executor
│   ├── rag.py           SentenceTransformers + FAISS schema index
│   ├── llm.py           Ollama client — returns structured JSON plan
│   └── requirements.txt
├── frontend/
│   ├── src/App.jsx      Complete React UI + Recharts
│   ├── index.html
│   ├── vite.config.js
│   └── package.json
└── README.md
```

---

## Supported operations

| What you ask | Operation |
|---|---|
| average / mean | aggregate → mean |
| total / sum | aggregate → sum |
| highest / most | sort + top_n |
| lowest / least | sort + top_n |
| filter / where | filter |
| over time | time_series → line chart |
| by device / by location | group_by → bar chart |
| compare X and Y | compare → scatter / bar |
| top N | top_n |

The LLM selects the chart type (bar, line, pie, scatter).
The backend **never executes arbitrary code** — only safe Pandas operations.
=======
# IOT_dataset_LLM
>>>>>>> e107ef0906ea4e03f4748710e39ea73e0df136cd

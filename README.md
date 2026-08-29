# 📡 IoT Data Intelligence Tool

> Ask questions about your IoT data in plain English — get charts, tables, and insights instantly.
> Runs **100% locally** using Hugging Face pretrained models. No Ollama. No API keys. No cloud.

---

## 🧠 How It Works

```
You type a question
        ↓
React UI sends it to FastAPI
        ↓
RAG (FAISS + SentenceTransformers) fetches relevant schema context
        ↓
Hugging Face LLM (Qwen2.5 / Phi-3 / Mistral) generates a JSON operation plan
        ↓
Pandas executes the plan safely (no arbitrary code execution)
        ↓
Result returned as Chart + Table to the UI
```

---

## 🗂️ Project Structure

```
iot-llm-tool/
├── backend/
│   ├── main.py            ← FastAPI app + safe Pandas executor
│   ├── rag.py             ← FAISS + SentenceTransformers schema index
│   ├── llm.py             ← Hugging Face local LLM (no API key, no Ollama)
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   └── App.jsx        ← Full React UI + Recharts
│   ├── index.html
│   ├── vite.config.js
│   └── package.json
├── .gitignore
└── README.md
```

---

## ⚙️ Prerequisites

Install these before running:

| Tool | Download |
|---|---|
| Python 3.10+ | https://www.python.org/downloads |
| Node.js (LTS) | https://nodejs.org |
| Git | https://git-scm.com |

> ❌ No Ollama needed. The model downloads automatically from Hugging Face.

---

## 🚀 How to Run

### Step 1 — Clone the repository

```bash
git clone https://github.com/yehaa2004/IOT_dataset_LLM.git
cd IOT_dataset_LLM
```

---

### Step 2 — Set up the Backend

```bash
cd backend
```

**Mac / Linux:**
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

**Windows (PowerShell):**
```powershell
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

> ⏳ **First run downloads the Hugging Face model (~3 GB). Just wait.**
> You will see `[llm] Model ready on cpu` when it is done.

---

### Step 3 — Set up the Frontend

Open a **new terminal window**:

```bash
cd IOT_dataset_LLM/frontend
npm install
npm run dev
```

---

### Step 4 — Open the App

```
http://localhost:5173
```

---

## 💬 Example Questions

Once you upload a CSV or Excel file, try asking:

- `Show average temperature for each device`
- `Which device consumed the most energy?`
- `Show energy consumption over time`
- `Devices with humidity above 70%`
- `Top 5 devices by energy consumption`
- `Compare temperature and humidity`

---

## 📁 Supported Data Format

Upload a **CSV or Excel** file with columns like:

```
timestamp, device_id, temperature, humidity, energy_consumption, location
```

Any structured IoT dataset works.

---

## 🤖 Changing the LLM Model

Edit `backend/llm.py` and change the `MODEL` variable:

```python
MODEL = "Qwen/Qwen2.5-1.5B-Instruct"           # Default (~3 GB, CPU friendly)
MODEL = "TinyLlama/TinyLlama-1.1B-Chat-v1.0"   # Fastest (~2 GB)
MODEL = "microsoft/Phi-3-mini-4k-instruct"      # Best quality (~7 GB)
MODEL = "mistralai/Mistral-7B-Instruct-v0.3"    # Highest quality (~14 GB, needs GPU)
MODEL = "google/gemma-2-2b-it"                  # Good balance (~5 GB)
```

Models are auto-downloaded on first run from Hugging Face. No installation needed.

---

## 🖥️ GPU Support (Optional — much faster)

If you have an NVIDIA GPU, install the CUDA version of PyTorch first:

```powershell
pip install torch --index-url https://download.pytorch.org/whl/cu121
pip install -r requirements.txt
```

The app automatically detects and uses your GPU.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React, Vite, Recharts |
| Backend | Python, FastAPI, Pandas |
| LLM | Hugging Face Transformers (runs locally) |
| RAG | Sentence Transformers + FAISS |
| No Ollama | ✅ |
| No API keys | ✅ |

---

## ⚠️ Notes

- The LLM **never executes arbitrary code** — it only returns a JSON plan that Pandas runs safely.
- Uploaded data files are excluded from Git via `.gitignore`.
- Model weights are excluded from Git and downloaded automatically on first run.

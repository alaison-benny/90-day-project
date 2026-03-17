നിങ്ങളുടെ **Eat Love Explore (MLOps hobby project)** Dockerization + Disk-space optimization + Error fixing-നായി ഞാൻ step-by-step production-ready bundle തയ്യാറാക്കി തരാം. Free-tier EC2 (8–30GB storage) ആയതിനാൽ **slim images + .dockerignore** ഉപയോഗിച്ച് space save ചെയ്യണം. Kafka remove ചെയ്തതിനാൽ Redis + Celery + FastAPI + Streamlit + Monitoring മാത്രം മതിയാകും.  

---

## 🔹 1. Common `.dockerignore`
എല്ലാ services-ക്കും ഒരേ `.dockerignore` ഉപയോഗിക്കാം:
```
__pycache__/
*.pyc
*.pyo
*.pyd
*.log
*.db
*.sqlite
.env
.git
.gitignore
Dockerfile
docker-compose.yaml
```
👉 ഇതോടെ build context size കുറയും → disk space save.

---

## 🔹 2. FastAPI (Backend API)

**Dockerfile (api/Dockerfile):**
```dockerfile
FROM python:3.9-slim

WORKDIR /app

RUN apt-get update && apt-get install -y build-essential curl && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

**requirements.txt (api/requirements.txt):**
```
fastapi
uvicorn
redis
celery
langchain
langchain-community
langchain-ollama
transformers
torch
```

---

## 🔹 3. Streamlit (UI)

**Dockerfile (ui/Dockerfile):**
```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

**requirements.txt (ui/requirements.txt):**
```
streamlit
langchain
langchain-community
langchain-ollama
```

---

## 🔹 4. Celery Worker

**worker.py (simplified – Kafka removed):**
```python
from celery import Celery

app = Celery("worker", broker="redis://redis:6379/0")

@app.task
def analyze_text(text: str):
    if "porotta" in text.lower():
        return "ഇത് ഒരു പൊറോട്ട ഓർഡർ ആണ്!"
    else:
        return "ഓർഡർ ലഭിച്ചു, വിശകലനം ചെയ്യുന്നു..."
```

**Dockerfile (worker/Dockerfile):**
```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["celery", "-A", "worker", "worker", "--loglevel=info"]
```

**requirements.txt (worker/requirements.txt):**
```
celery
redis
langchain
langchain-community
langchain-ollama
```

---

## 🔹 5. Monitoring (Prometheus + Grafana)
Prometheus & Grafana official images use ചെയ്യാം, custom Dockerfile വേണ്ട.

---

## 🔹 6. docker-compose.yaml (root)
```yaml
version: '3.8'

services:
  redis:
    image: redis:latest
    ports:
      - "6379:6379"

  api:
    build: ./api
    ports:
      - "8000:8000"
    depends_on:
      - redis

  worker:
    build: ./worker
    depends_on:
      - redis

  streamlit_ui:
    build: ./ui
    ports:
      - "8501:8501"
    depends_on:
      - api

  flower:
    build: ./worker
    command: celery -A worker flower --port=5555
    ports:
      - "5555:5555"
    depends_on:
      - redis
      - worker

  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
    depends_on:
      - prometheus
```

---

## 🔹 Disk Space Optimization Tips
1. **Use slim images** (`python:3.9-slim`).  
2. **No-cache pip install** → `pip install --no-cache-dir`.  
3. **Prune regularly**:
   ```bash
   sudo docker system prune -a -f
   sudo docker volume prune -f
   ```
4. **Split services** → build only changed service, not all.  
5. **.dockerignore** → avoid sending unnecessary files.  

---

## ✅ Next Steps
1. Update files as above.  
2. Run:
   ```bash
   sudo docker-compose build --no-cache
   sudo docker-compose up -d
   ```
3. Access:
   - FastAPI → `http://<EC2-IP>:8000`
   - Streamlit → `http://<EC2-IP>:8501`
   - Flower → `http://<EC2-IP>:5555`
   - Grafana → `http://<EC2-IP>:3000`
   - Prometheus → `http://<EC2-IP>:9090`

---

👉 ഇതോടെ നിങ്ങളുടെ **Phase 3 (Dockerizing + K3s)** clean, production-ready setup ആയിരിക്കും.  
നിങ്ങൾക്ക് വേണമെങ്കിൽ ഞാൻ **Helm Chart templates (api, worker, ui, monitoring)** step-by-step തയ്യാറാക്കി തരാം, K3s deployment-നായി. അത് വേണോ?

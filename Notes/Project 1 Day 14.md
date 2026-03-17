ചേട്ടായി ❤️ — perfect! ഇനി **Project 3: Model Serving API with FastAPI + Docker + AWS** തുടങ്ങാം.  

---

## 🔹 Project Roadmap (Step‑by‑Step)
ഞങ്ങൾ stages + extra steps combine ചെയ്ത് production‑ready workflow build ചെയ്യാം:

### **Stage 1: Python Basics Refresh**
- Create virtual environment:
  ```bash
  python3 -m venv venv
  source venv/bin/activate
  ```
- Add dependencies in `requirements.txt`:
  ```
  fastapi
  uvicorn
  scikit-learn
  transformers
  joblib
  ```

---

### **Stage 2: Simple ML Model (Sentiment Analysis)**
- Train a basic sentiment classifier:
  ```python
  from sklearn.feature_extraction.text import CountVectorizer
  from sklearn.naive_bayes import MultinomialNB
  import joblib

  texts = ["I love this!", "This is bad", "Amazing work", "Terrible experience"]
  labels = [1, 0, 1, 0]

  vectorizer = CountVectorizer()
  X = vectorizer.fit_transform(texts)

  model = MultinomialNB()
  model.fit(X, labels)

  joblib.dump(model, "sentiment_model.pkl")
  joblib.dump(vectorizer, "vectorizer.pkl")
  ```

---

### **Stage 3: FastAPI Basics**
- Create `app.py`:
  ```python
  from fastapi import FastAPI
  import joblib

  app = FastAPI()
  model = joblib.load("sentiment_model.pkl")
  vectorizer = joblib.load("vectorizer.pkl")

  @app.get("/")
  def root():
      return {"status": "ok"}

  @app.post("/predict")
  def predict(text: str):
      X = vectorizer.transform([text])
      prediction = model.predict(X)[0]
      return {"sentiment": "positive" if prediction == 1 else "negative"}
  ```

---

### **Stage 4: Containerization**
- Create `Dockerfile`:
  ```dockerfile
  FROM python:3.10-slim
  WORKDIR /app
  COPY requirements.txt .
  RUN pip install -r requirements.txt
  COPY . .
  CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
  ```
- Build & run:
  ```bash
  docker build -t sentiment-api .
  docker run -p 8000:8000 sentiment-api
  ```

---

### **Stage 5: AWS EC2 Setup**
- Launch EC2 instance (Ubuntu 22.04).  
- Configure **Security Group** → allow inbound on port 8000.  
- SSH into EC2:
  ```bash
  ssh -i mykey.pem ubuntu@<EC2-Public-IP>
  ```
- Install Docker:
  ```bash
  sudo apt update
  sudo apt install docker.io -y
  ```

---

### **Stage 6: Deploy Dockerized App**
- Copy project files to EC2 (`scp`).  
- Build Docker image on EC2:
  ```bash
  docker build -t sentiment-api .
  docker run -d -p 8000:8000 sentiment-api
  ```
- Test API:
  ```
  http://<EC2-Public-IP>:8000/predict?text=I love this
  ```

---

### **Stage 7: Logging + Monitoring**
- Add Python logging in `app.py`:
  ```python
  import logging
  logging.basicConfig(level=logging.INFO)
  logger = logging.getLogger(__name__)
  ```
- AWS CloudWatch → configure Docker logs shipping.  
- Prometheus scrape config → `prometheus.yml` add target EC2 IP.  
- Grafana → create dashboard for request latency, error rate.

---

### **Stage 8: Documentation**
- Create `README.md` → project overview, setup steps.  
- Add architecture diagram (FastAPI → Docker → EC2 → Prometheus/Grafana).  
- Push repo to GitHub:
  ```bash
  git init
  git remote add origin <repo-url>
  git push -u origin main
  ```

---

## ✅ Takeaway
- We’ll build a **sentiment analysis API** with FastAPI.  
- Containerize with Docker.  
- Deploy to AWS EC2.  
- Add logging + monitoring.  
- Document + push to GitHub.  

---

ചേട്ടായി, immediate step:  
👉 Start with **Stage 1: Python basics refresh** — create `venv` + `requirements.txt`.  
👉 ഞാൻ ready‑to‑copy `requirements.txt` content തരട്ടേ?

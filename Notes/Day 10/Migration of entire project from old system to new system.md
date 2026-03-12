ചേട്ടായി 👍 — നല്ല തീരുമാനമാണ്. EC2 instance‑ൽ disk space 90% full ആണെങ്കിൽ, project run ചെയ്യുന്നത് unstable ആയിരിക്കും. പുതിയ **8GB RAM + 600GB storage EC2**‑ൽ migrate ചെയ്യുന്നത് best solution ആണ്.  

---

## 🔹 Step‑by‑Step Migration Process

### Step 1: Prepare new EC2
1. Launch new EC2 (Ubuntu preferred).  
2. Install required packages:
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install -y docker.io docker-compose helm kubectl
   ```
3. Install k3s:
   ```bash
   curl -sfL https://get.k3s.io | sh -
   ```

---

### Step 2: Copy project files
On old EC2:
```bash
cd ~/Langchain-Chatchat
tar -czvf project_backup.tar.gz docker ai_worker_project fastapi-chart
```

Copy to new EC2 (replace `<NEW_IP>`):
```bash
scp -i <your-key.pem> project_backup.tar.gz ubuntu@<NEW_IP>:/home/ubuntu/
```

On new EC2:
```bash
tar -xzvf project_backup.tar.gz
```

---

### Step 3: Setup Python environment
```bash
cd ~/Langchain-Chatchat/docker/ai_worker_project
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

---

### Step 4: Pull Docker images
Since image already pushed to Docker Hub:
```bash
docker pull alaisonbenny/ai_worker_project_api:latest
```

---

### Step 5: Redeploy Helm chart
```bash
cd ~/Langchain-Chatchat/docker/ai_worker_project/fastapi-chart
helm upgrade --install fastapi .
```

---

### Step 6: Verify pods
```bash
kubectl get pods
kubectl logs <pod-name>
```

---

### Step 7: Clean old EC2 (optional)
Stop services and free resources:
```bash
sudo systemctl stop k3s
sudo shutdown -h now
```

---

## ✅ Takeaway
- Old EC2 → backup project files.  
- New EC2 → install Docker, k3s, Helm, Python.  
- Copy project → setup venv → pull image from Docker Hub.  
- Redeploy Helm → continue from where stopped.  

---

ചേട്ടായി 👍 — ഇപ്പോൾ situation: പഴയ EC2‑ൽ disk space 100% full ആയി, backup tar.gz create ചെയ്യുമ്പോൾ fail ചെയ്തു. ഇപ്പോൾ 80% free space കാണിച്ചാലും, വലിയ project archive ചെയ്യുമ്പോൾ വീണ്ടും full ആകും.  

---

## 🔹 Safe Migration Strategy (Disk Full Avoidance)

### Option 1: Direct `scp` (Best)
Instead of creating `.tar.gz` locally (which consumes disk space), directly copy project folder to new EC2:

On **new EC2** (replace `<OLD_IP>` and `<KEY>`):
```bash
scp -i <KEY.pem> -r ubuntu@<OLD_IP>:/home/ubuntu/Langchain-Chatchat ~/Langchain-Chatchat
```

👉 ഇത് tar file create ചെയ്യാതെ, files stream ആയി transfer ചെയ്യും → disk space waste ഇല്ല.

---

### Option 2: Rsync (Efficient + Resume Support)
```bash
rsync -avz -e "ssh -i <KEY.pem>" ubuntu@<OLD_IP>:/home/ubuntu/Langchain-Chatchat ~/Langchain-Chatchat
```
- `-a` → preserve permissions  
- `-v` → verbose  
- `-z` → compress during transfer  
- Resume support if connection breaks.

---

### Option 3: GitHub / Remote Repo (If project already versioned)
- Push project code to GitHub (only source files, not venv).  
- Clone directly on new EC2.  
- Docker images already in Docker Hub → no need to copy.

---

## 🔹 What NOT to Copy
- Avoid copying `venv/` and `.tar.gz` (huge, unnecessary).  
- Only copy:
  - `docker/ai_worker_project/` (code, configs, CSV, requirements.txt)  
  - `fastapi-chart/` (Helm chart)  
  - Terraform files (if infra automation needed)

👉 Python venv + Docker images can be rebuilt fresh on new EC2.

---

## ✅ Migration Steps for ചേട്ടായി
1. On new EC2 → install Docker, Helm, k3s, Python.  
2. Use **scp/rsync** to copy project folder directly (no tar).  
3. Recreate venv:
   ```bash
   cd ~/Langchain-Chatchat/docker/ai_worker_project
   python3 -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```
4. Pull Docker Hub image:
   ```bash
   docker pull alaisonbenny/ai_worker_project_api:latest
   ```
5. Redeploy Helm chart:
   ```bash
   cd fastapi-chart
   helm upgrade --install fastapi .
   ```
6. Verify pods:
   ```bash
   kubectl get pods
   kubectl logs <pod-name>
   ```

---

## 🔹 Takeaway
- Disk full issue = tar.gz creation locally.  
- Fix = **stream copy with scp/rsync** → no local archive.  
- Only copy essential project files → rebuild venv + pull image fresh.  
- New EC2 (8GB RAM + 600GB disk) → stable, production‑ready.

---

ചേട്ടായി, immediate solution:  
```bash
scp -i <KEY.pem> -r ubuntu@<OLD_IP>:/home/ubuntu/Langchain-Chatchat ~/Langchain-Chatchat
```

👉 ഇതോടെ project directly പുതിയ EC2‑ലേക്ക് copy ചെയ്യും, disk space issue ഇല്ല.  

നിനക്ക് ഞാൻ **exact rsync command with resume support** ready‑to‑copy തരട്ടേ?

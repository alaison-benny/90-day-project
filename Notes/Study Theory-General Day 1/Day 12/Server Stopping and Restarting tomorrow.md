ചേട്ടായി 👍 — എല്ലാം ഇപ്പോൾ ശരിയായി work ചെയ്യുന്നുണ്ട്. ഇനി server close ചെയ്ത്, നാളെ രാവിലെ വീണ്ടും restart ചെയ്യാൻ വേണ്ട steps ഞാൻ step‑by‑step തരാം.  

---

## 🔹 Server close ചെയ്യുന്നത്
1. **Local uvicorn run ചെയ്താൽ**  
   - Terminal‑ൽ run ചെയ്യുന്ന uvicorn server stop ചെയ്യാൻ →  
     ```bash
     CTRL + C
     ```
   👉 ഇതോടെ FastAPI server stop ചെയ്യും.

2. **Kubernetes pods run ചെയ്യുന്നുണ്ടെങ്കിൽ**  
   - Pods stop ചെയ്യേണ്ടതില്ല, കാരണം Kubernetes auto‑manage ചെയ്യും.  
   - Learning project‑ൽ pods Running status‑ൽ തന്നെ വിടാം.  
   - Server close ചെയ്യാൻ EC2 instance‑ൽ നിന്ന് logout ചെയ്യുക:
     ```bash
     exit
     ```

---

## 🔹 Server restart ചെയ്യുന്നത് (Tomorrow Morning)
1. **Virtual environment activate ചെയ്യുക**
   ```bash
   cd ~/project2-ml-api
   source venv/bin/activate
   ```

2. **Local uvicorn run ചെയ്യാൻ (testing only)**
   ```bash
   uvicorn app:app --host 0.0.0.0 --port 8000
   ```
   👉 Browser‑ൽ:
   ```
   http://127.0.0.1:8000/predict?area=1200&bedrooms=3&age=10
   ```

3. **Kubernetes restart ചെയ്യാൻ**
   - Pods already Running ആണെങ്കിൽ restart വേണ്ട.  
   - Deployment restart ചെയ്യാൻ:
     ```bash
     kubectl rollout restart deployment house-price-api-deployment
     kubectl get pods
     ```
   👉 Pods Running status confirm ചെയ്യുക.

4. **Service access ചെയ്യാൻ**
   - NodePort URL check ചെയ്യാൻ:
     ```bash
     minikube service house-price-api-service --url
     ```
   👉 Curl run ചെയ്യുക:
     ```bash
     curl "http://192.168.49.2:30008/predict?area=1200&bedrooms=3&age=10"
     ```

---

## 🔹 Real‑Life Production Parallel
- **Airbnb** → Developers dev cluster‑ൽ pods Running വിടുന്നു, രാവിലെ വീണ്ടും login ചെയ്ത് kubectl rollout restart run ചെയ്യുന്നു.  
- **Spotify** → Local dev servers uvicorn stop ചെയ്ത്, next day വീണ്ടും run ചെയ്യുന്നു.  
- **Netflix** → Production pods auto‑restart ചെയ്യുന്നു, manual restart വേണ്ട.  

---

## ✅ Takeaway
- Server close → CTRL+C (uvicorn) അല്ലെങ്കിൽ logout (EC2).  
- Restart → venv activate + uvicorn run OR kubectl rollout restart.  
- Service access → minikube service command + curl/browser.  

---

ചേട്ടായി, immediate step:  
👉 ഇപ്പോൾ `CTRL+C` press ചെയ്ത് uvicorn stop ചെയ്യൂ, EC2‑ൽ നിന്ന് logout ചെയ്യൂ.  
👉 നാളെ രാവിലെ `source venv/bin/activate` + `kubectl rollout restart` run ചെയ്താൽ Stage 4 + Stage 5 തുടങ്ങാം.

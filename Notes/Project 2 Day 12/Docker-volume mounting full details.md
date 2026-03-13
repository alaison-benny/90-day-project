ചേട്ടായി 👍 — model.pkl Docker image‑ൽ embed ചെയ്യാതെ **volume mount** ചെയ്യുന്നത് നല്ല practice ആണ്. Production‑ൽ പല കമ്പനികളും (Airbnb, Spotify, Netflix) model files external storage (S3, GCS, PVC)‑ൽ store ചെയ്ത് pod‑ൽ mount ചെയ്യാറുണ്ട്.  

---

## 🔹 Volume concept
- **Docker/Kubernetes volume** → container restart ചെയ്താലും data persist ചെയ്യും.  
- **Free?** → Docker volume host machine disk space ഉപയോഗിക്കുന്നു. Free അല്ല, host disk quota അനുസരിച്ച് limit.  
- **Maximum size?** → Limit = host machine disk capacity. ഉദാ: EC2 instance 20GB disk ഉണ്ടെങ്കിൽ, volume‑ൽ 20GB വരെ store ചെയ്യാം.  
- **What can be mounted?**  
  - Files (pickle, config, secrets).  
  - Directories (datasets, logs).  
  - External storage (AWS EBS, S3 via CSI driver).  

---

## 🔹 DockerHub relation
- **Image push** → only container filesystem push ചെയ്യും.  
- **Volume data** → DockerHub‑ൽ push ചെയ്യില്ല.  
- Volume host machine‑ൽ അല്ലെങ്കിൽ Kubernetes PersistentVolume‑ൽ separate manage ചെയ്യണം.  
- **When pulling image** → only image layers pull ചെയ്യും, volume data separately attach ചെയ്യണം.

---

## 🔹 Step‑by‑Step Process (Malayalam)

### 1. Model file prepare ചെയ്യുക
```bash
ls model.pkl   # confirm file exists
```

### 2. Kubernetes ConfigMap/PersistentVolume create ചെയ്യുക
**Option A: ConfigMap (small files like pickle)**
```bash
kubectl create configmap house-price-model --from-file=model.pkl
```

### 3. Deployment.yaml update ചെയ്യുക
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: house-price-api-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: house-price-api
  template:
    metadata:
      labels:
        app: house-price-api
    spec:
      containers:
      - name: house-price-api
        image: alaisonbenny/house-price-api:latest
        ports:
        - containerPort: 8000
        volumeMounts:
        - name: model-volume
          mountPath: /app/model.pkl
          subPath: model.pkl
      volumes:
      - name: model-volume
        configMap:
          name: house-price-model
```

### 4. Apply ചെയ്യുക
```bash
kubectl apply -f Deployment.yaml
kubectl get pods
```

### 5. Service.yaml apply ചെയ്യുക
```bash
kubectl apply -f Service.yaml
kubectl get svc
```

### 6. Access ചെയ്യുക
```bash
http://<Node-IP>:30008/predict?area=1200&bedrooms=3&age=10
```

---

## ✅ Takeaway
- **Volume mount** → model.pkl DockerHub‑ൽ push ചെയ്യേണ്ടതില്ല.  
- Image pull ചെയ്താൽ → pod start സമയത്ത് ConfigMap/Volume attach ചെയ്യും.  
- Production‑ൽ → companies model files PVC/S3‑ൽ store ചെയ്ത് pods‑ൽ mount ചെയ്യുന്നു.  

---

ചേട്ടായി, immediate step:  
👉 `kubectl create configmap house-price-model --from-file=model.pkl` run ചെയ്ത്, Deployment.yaml‑ൽ volumeMount add ചെയ്യൂ.  
അതിനുശേഷം pods restart ചെയ്താൽ CrashLoopBackOff disappear ചെയ്യും.  

ന്റെ ചേട്ടായിക്ക് ഞാൻ **ready‑to‑copy Service.yaml** തരട്ടേ, NodePort വഴി API access ചെയ്യാൻ?

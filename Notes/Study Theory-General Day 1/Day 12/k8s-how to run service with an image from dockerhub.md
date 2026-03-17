ചേട്ടായി 👍 — ഇനി **FastAPI ML API Deployment** Kubernetes‑ൽ apply ചെയ്യാം. Docker Hub‑ൽ push ചെയ്ത image (`alaisonbenny/house-price-api:latest`) ഉപയോഗിച്ച് cluster‑ൽ run ചെയ്യാം.  

---

## 📜 Deployment.yaml
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
        resources:
          requests:
            cpu: "250m"
            memory: "256Mi"
          limits:
            cpu: "500m"
            memory: "512Mi"
```

---

## 📜 Service.yaml
```yaml
apiVersion: v1
kind: Service
metadata:
  name: house-price-api-service
spec:
  type: NodePort
  selector:
    app: house-price-api
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8000
      nodePort: 30008
```

---

## 🔹 Apply ചെയ്യാനുള്ള Commands
```bash
kubectl apply -f Deployment.yaml
kubectl apply -f Service.yaml
```

Check status:
```bash
kubectl get pods
kubectl get svc
kubectl get nodes -o wide
```

---

## 🔹 Access ചെയ്യുക
- Node IP കണ്ടെത്തുക:
  ```bash
  kubectl get nodes -o wide
  ```
- Browser/`curl` വഴി access ചെയ്യുക:
  ```
  http://<Node-IP>:30008/predict?area=1200&bedrooms=3&age=10
  ```
👉 Output JSON → `{"predicted_price": ...}`

---

## ✅ Takeaway
- Nginx practice → Deployment + Service basics clear.  
- ഇപ്പോൾ FastAPI ML API Docker Hub image Kubernetes‑ൽ run ചെയ്തു.  
- NodePort വഴി external access → ML model predictions ലഭിക്കും.  

---

ചേട്ടായി, immediate step:  
👉 `Deployment.yaml` + `Service.yaml` save ചെയ്ത് apply ചെയ്യൂ.  
അതിനുശേഷം ഞാൻ **Horizontal Pod Autoscaler (HPA) YAML** ready‑to‑copy തരാം, load‑നുസരിച്ച് pods auto‑scale ചെയ്യാൻ.

ചേട്ടായി 👍 — ഇപ്പോൾ pods എല്ലാം Running ആണ്, curl വഴി API response കിട്ടുന്നു. Browser‑ൽ access ചെയ്യാൻ Ingress/DNS setup വേണം, പക്ഷേ **learning project**‑ൽ അത് skip ചെയ്ത് next stage‑ലേക്ക് പോകാം. Production‑ൽ Airbnb, Spotify, Netflix പോലുള്ള കമ്പനികൾ Ingress + DNS + SSL configure ചെയ്യുന്നു, പക്ഷേ learning‑ൽ NodePort + curl മതിയാകും.  

---

## 🔹 Next Stage (ഘട്ടം 4: Deployment + HPA)

### 1. Deployment.yaml
- Pod replicas define ചെയ്യുന്നു.  
- Labels → Service selector match ചെയ്യണം.  
- Example:
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
            cpu: 250m
            memory: 512Mi
          limits:
            cpu: 1
            memory: 1Gi
```

### 2. Service.yaml
- NodePort service already ഉണ്ടു.  
- TargetPort = 8000, NodePort = 30008.

### 3. Horizontal Pod Autoscaler (HPA)
- Traffic load കൂടുമ്പോൾ pods auto‑scale ചെയ്യണം.  
- Example:
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: house-price-api-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: house-price-api-deployment
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

👉 ഇതോടെ CPU usage 50% cross ചെയ്താൽ pods 2 → 10 വരെ auto‑scale ചെയ്യും.

---

## 🔹 ഘട്ടം 5: Load Balancing & Testing
- **AWS Load Balancer** → Service type LoadBalancer configure ചെയ്യുക.  
- **Locust/JMeter** → concurrent requests simulate ചെയ്ത് scaling verify ചെയ്യുക.  
- Real‑life example:  
  - **Airbnb** → Kubernetes HPA + AWS ALB ഉപയോഗിച്ച് pricing API scale ചെയ്യുന്നു.  
  - **Spotify** → Recommendation API load test Locust ഉപയോഗിച്ച് ചെയ്യുന്നു.  
  - **Netflix** → JMeter load test run ചെയ്ത് scaling efficiency verify ചെയ്യുന്നു.

---

## ✅ Takeaway
- Ingress/DNS skip ചെയ്ത് learning project‑ൽ next stage move ചെയ്യാം.  
- Stage 4 → Deployment + Service + HPA setup.  
- Stage 5 → AWS Load Balancer + Locust/JMeter load test.  

---

ചേട്ടായി, immediate step:  
👉 ഞാൻ ready‑to‑copy Deployment.yaml + HPA.yaml തരാം.  
അതിനുശേഷം ചേട്ടായി `kubectl apply -f` run ചെയ്ത് pods auto‑scale ചെയ്യുന്നുണ്ടോ എന്ന് verify ചെയ്യാം.  

ചേട്ടായി, Stage 4‑ൽ Deployment + HPA YAML full example വേണോ?

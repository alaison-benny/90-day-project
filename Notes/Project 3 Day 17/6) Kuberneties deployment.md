
### **അടുത്ത ദൗത്യം: Kubernetes Deployment & Service**

കുബെർനെറ്റീസിൽ ഒരു ആപ്പ് റൺ ചെയ്യാൻ നമുക്ക് രണ്ട് കാര്യങ്ങൾ വേണം:
1.  **Deployment:** നമ്മുടെ കണ്ടെയ്നർ എത്ര എണ്ണം വേണം, ഏത് ഇമേജ് ഉപയോഗിക്കണം എന്ന് തീരുമാനിക്കാൻ.
2.  **Service:** പുറംലോകത്തിന് (നമുക്ക്) ഈ ആപ്പിലേക്ക് കണക്ട് ചെയ്യാൻ ഒരു വഴി ഉണ്ടാക്കാൻ.



---

### **നമുക്ക് ആദ്യത്തെ YAML ഫയൽ എഴുതാം**

**ചേട്ടായി** ഒരു പുതിയ ഫയൽ ഉണ്ടാക്കൂ:
```bash
vim deployment.yaml
```

എന്നിട്ട് താഴെ കാണുന്ന കോഡ് അതിൽ പേസ്റ്റ് ചെയ്യുക. (ഇവിടെ നമ്മൾ നേരത്തെ ഉണ്ടാക്കിയ `mlops-api:v3` എന്ന ഇമേജ് ആണ് ഉപയോഗിക്കുന്നത്).

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mlops-deployment
spec:
  replicas: 1 # എത്ര കണ്ടെയ്നർ വേണം
  selector:
    matchLabels:
      app: mlops-api
  template:
    metadata:
      labels:
        app: mlops-api
    spec:
      containers:
      - name: mlops-container
        image: mlops-api:v3
        imagePullPolicy: Never # ലോക്കൽ ഇമേജ് ആയതുകൊണ്ട് ഇത് നിർബന്ധമാണ്
        ports:
        - containerPort: 8000
        env:
        - name: MODEL_PATH
          value: "/app/mlruns/1/models/m-553ae89e3eb44e41a87c2cbcca7bc524/artifacts"
---
apiVersion: v1
kind: Service
metadata:
  name: mlops-service
spec:
  selector:
    app: mlops-api
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8000
  type: NodePort
```

---

### **ഒരു ചെറിയ ടെക്നിക്കൽ കാര്യം (പ്ലാറ്റ്‌ഫോം എൻജിനീയർ ശ്രദ്ധിക്കാൻ):**

നമ്മൾ ഇമേജ് ബിൽഡ് ചെയ്തത് ലോക്കൽ ഡോക്കറിലാണ്. മിനികൂബിന് ആ ഇമേജ് കാണണമെങ്കിൽ ഒന്നുകിൽ അത് ക്ലൗഡിൽ (ECR) ആയിരിക്കണം, അല്ലെങ്കിൽ ലോക്കൽ ഡോക്കർ ഇമേജിനെ മിനികൂബിലേക്ക് പുഷ് ചെയ്യണം.

നമുക്ക് ഇപ്പോൾ എളുപ്പത്തിന് ഈ കമാൻഡ് ഒന്ന് അടിക്കാം. ഇത് ലോക്കൽ ഇമേജിനെ മിനികൂബിലേക്ക് ലോഡ് ചെയ്യും:
```bash
minikube image load mlops-api:v3
```

അതിനുശേഷം ഈ കമാൻഡ് അടിച്ച് നമ്മുടെ ആപ്പിനെ കുബെർനെറ്റീസിലേക്ക് വിടാം:
```bash
kubectl apply -f deployment.yaml
```

**ന്റെ ചേട്ടായി**, ഈ സ്റ്റെപ്പുകൾ ഒന്ന് ചെയ്തു നോക്കൂ. എന്നിട്ട് **`kubectl get pods`** എന്ന് അടിച്ചു നോക്കുമ്പോൾ അവിടെ `Running` എന്ന് വരുന്നുണ്ടോ എന്ന് പറയൂ.

ആദ്യത്തെ ഘട്ടം **Deployment (Kubernetes Manifests)** ആണ്. ഇതിൽ നമുക്ക് എങ്ങനെ ഒരു ആപ്ലിക്കേഷൻ കുബെർനെറ്റിസിലേക്ക് കൊണ്ടുവരാമെന്നും അതിലെ സെറ്റിംഗ്‌സ് എങ്ങനെ മാനേജ് ചെയ്യാമെന്നും നോക്കാം.

---

## Phase 1: Deployment (Manifests, ConfigMaps, Secrets)

ഒരു ആപ്ലിക്കേഷൻ EKS-ൽ റൺ ചെയ്യണമെങ്കിൽ നമുക്ക് പ്രധാനമായും മൂന്ന് തരം YAML ഫയലുകൾ ആവശ്യമാണ്:

### 1. Deployment YAML
ഇതാണ് നമ്മുടെ ആപ്ലിക്കേഷന്റെ 'മാസ്റ്റർ പ്ലാൻ'. എത്ര കോപ്പികൾ (Pods) വേണം, ഏത് ഇമേജ് ഉപയോഗിക്കണം തുടങ്ങിയ കാര്യങ്ങൾ ഇതിലാണ് പറയുന്നത്.

### 2. ConfigMaps (Environment Configs)
നമ്മുടെ ആപ്പിലെ ഡാറ്റാബേസ് URL അല്ലെങ്കിൽ മറ്റ് സ്റ്റാറ്റിക് ആയ കാര്യങ്ങൾ (ഉദാഹരണത്തിന് `LOG_LEVEL=info`) ആപ്പിന്റെ കോഡിനുള്ളിൽ എഴുതാതെ പുറത്ത് സൂക്ഷിക്കാൻ ഇത് സഹായിക്കുന്നു.

### 3. Secrets (Secrets Management)
പാസ്‌വേഡുകൾ, API കീകൾ തുടങ്ങിയ രഹസ്യ വിവരങ്ങൾ സൂക്ഷിക്കാനാണ് ഇത് ഉപയോഗിക്കുന്നത്.

> **Real-world Insight: AWS Secrets Manager vs Kubernetes Secrets**
> * **Kubernetes Secrets:** ഇത് ക്ലസ്റ്ററിനുള്ളിൽ തന്നെ ഉണ്ടാക്കുന്നതാണ്. പക്ഷേ ഇത് 'Base64' എൻകോഡിംഗ് മാത്രമാണ് ചെയ്യുന്നത് (ശരിക്കുള്ള എൻക്രിപ്ഷൻ അല്ല).
> * **AWS Secrets Manager:** വലിയ കമ്പനികൾ ഇത് ഉപയോഗിക്കും. കാരണം ഇതിന് കൂടുതൽ സുരക്ഷയുണ്ട്, കീകൾ ഓട്ടോമാറ്റിക്കായി റൊട്ടേറ്റ് ചെയ്യാൻ സാധിക്കും. EKS-ൽ **External Secrets Operator (ESO)** ഉപയോഗിച്ച് ഈ കീകൾ നമുക്ക് ആപ്പിലേക്ക് കൊണ്ടുവരാം.

---

### **നമുക്ക് ഇതൊന്നു ചെയ്തു നോക്കാം (Hands-on)**

ആദ്യം ചേട്ടായിയുടെ ആപ്പിന് വേണ്ടിയുള്ള ഒരു `Deployment` ഫയൽ ഉണ്ടാക്കാം. ചേട്ടായിയുടെ പ്രോജക്ട് ഫോൾഡറിൽ `k8s` എന്നൊരു പുതിയ ഫോൾഡർ ഉണ്ടാക്കി അതിൽ `deployment.yaml` എന്ന ഫയൽ ക്രിയേറ്റ് ചെയ്യുക.



**`deployment.yaml`**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sentiment-api
  labels:
    app: sentiment-api
spec:
  replicas: 2
  selector:
    matchLabels:
      app: sentiment-api
  template:
    metadata:
      labels:
        app: sentiment-api
    spec:
      containers:
      - name: api-container
        image: <your-docker-hub-image-url> # ചേട്ടായിയുടെ ഇമേജ് ഇവിടെ നൽകണം
        ports:
        - containerPort: 80
        envFrom:
        - configMapRef:
            name: api-config
        - secretRef:
            name: api-secrets
```

**`configmap.yaml`**
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: api-config
data:
  APP_ENV: "production"
  DB_HOST: "eks-db-instance"
```

**`secrets.yaml`** (ഇത് തൽക്കാലം ക്ലസ്റ്റർ സീക്രട്ട് ആയി ചെയ്യാം)
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: api-secrets
type: Opaque
data:
  DB_PASSWORD: bXlwYXNzd29yZA== # 'mypassword' എന്നതിന്റെ Base64 രൂപം
```

---

### **അടുത്ത പടി:**
ഈ ഫയലുകൾ അപ്ലൈ ചെയ്യാൻ താഴെ പറയുന്ന കമാൻഡ്സ് ഉപയോഗിക്കാം:

1. `kubectl apply -f configmap.yaml`
2. `kubectl apply -f secrets.yaml`
3. `kubectl apply -f deployment.yaml`


--------------------
ശരി ചേട്ടായി! നമുക്ക് ആ പാസ്‌വേഡ് (`mypassword` - അതായത് `bXlwYXNzd29yZA==`) വെച്ച് തന്നെ മുന്നോട്ട് പോകാം. പഠിക്കുന്ന സമയത്ത് നമുക്ക് ലളിതമായ കാര്യങ്ങൾ ഉപയോഗിക്കുന്നതാണ് എളുപ്പം.

നമ്മൾ ഇപ്പോൾ **Phase 1 (Deployment)** ഏകദേശം പൂർത്തിയാക്കി. ഇനി ഈ ഫയലുകൾ ഉപയോഗിച്ച് എങ്ങനെ ആപ്പ് ഡിപ്ലോയ് ചെയ്യാം എന്ന് നോക്കാം. ചേട്ടായി താഴെ പറയുന്ന സ്റ്റെപ്പുകൾ ഒന്ന് ചെയ്യുമോ?

### 1. മാനിഫെസ്റ്റ് ഫയലുകൾ ക്രിയേറ്റ് ചെയ്യുക
ചേട്ടായിയുടെ ടെർമിനലിൽ `k8s` എന്നൊരു ഫോൾഡർ ഉണ്ടാക്കി അതിനുള്ളിൽ ഈ മൂന്ന് ഫയലുകൾ സേവ് ചെയ്യുക.

* **`configmap.yaml`**
* **`secrets.yaml`**
* **`deployment.yaml`** (ഇതിൽ `image:` എന്ന ഭാഗത്ത് ചേട്ടായിയുടെ Docker Image URL നൽകാൻ മറക്കരുത്. ഇല്ലെങ്കിൽ `nginx` എന്ന് തൽക്കാലം നൽകാം).



### 2. ഫയലുകൾ ക്ലസ്റ്ററിലേക്ക് അപ്‌ലൈ ചെയ്യുക
താഴെ പറയുന്ന കമാൻഡുകൾ ഓരോന്നായി അടിക്കുക:

```bash
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/secrets.yaml
kubectl apply -f k8s/deployment.yaml
```

### 3. സ്റ്റാറ്റസ് ചെക്ക് ചെയ്യുക
ആപ്പ് റെഡിയായോ എന്ന് നോക്കാൻ ഇത് അടിക്കൂ:
```bash
kubectl get pods
```
രണ്ട് പോഡുകളും (Replicas: 2) **`Running`** സ്റ്റേറ്റിൽ വരണം.

---

## Phase 2: Scaling (Horizontal Pod Autoscaler - HPA)

ഇനി നമുക്ക് രസകരമായ ഭാഗത്തേക്ക് കടക്കാം. ചേട്ടായിയുടെ ആപ്പിലേക്ക് ഒരുപാട് ആളുകൾ ഒരേസമയം കയറിയാൽ (High Load), കുബെർനെറ്റിസ് എങ്ങനെയാണ് തനിയെ കൂടുതൽ പോഡുകൾ ഉണ്ടാക്കുന്നത് (Scale-out) എന്ന് നോക്കാം.

### **എന്താണ് HPA?**
നമ്മുടെ സെർവറിലെ CPU ഉപയോഗം (ഉദാഹരണത്തിന് 50% കൂടുതൽ) കൂടിയാൽ, HPA അത് തിരിച്ചറിയുകയും ഉടൻ തന്നെ പുതിയ പോഡുകൾ ഉണ്ടാക്കി ലോഡ് കുറയ്ക്കുകയും ചെയ്യും. ലോഡ് കുറയുമ്പോൾ അത് തനിയെ പോഡുകൾ കുറയ്ക്കുകയും ചെയ്യും (Scale-in).



### **ഇത് ചെയ്യാനായി നമുക്ക് രണ്ട് കാര്യങ്ങൾ വേണം:**

1.  **Metrics Server:** ക്ലസ്റ്ററിലെ ഓരോ പോഡും എത്ര CPU ഉപയോഗിക്കുന്നു എന്ന് അളക്കാൻ ഈ സർവർ ആവശ്യമാണ്. (EKS-ൽ ഇത് നമ്മൾ ഇൻസ്റ്റാൾ ചെയ്യണം).
2.  **Resource Limits:** നമ്മുടെ `deployment.yaml`-ൽ ആപ്പിന് എത്ര CPU വേണമെന്ന് കൃത്യമായി പറഞ്ഞു കൊടുക്കണം. എങ്കിൽ മാത്രമേ HPA-ക്ക് അത് അളക്കാൻ പറ്റൂ.

---

ആ മൂന്ന് ഫയലുകൾ അപ്‌ലൈ ചെയ്തോ?** അത് കഴിഞ്ഞാൽ നമുക്ക് **Metrics Server** ഇൻസ്റ്റാൾ ചെയ്യാനും ആപ്പ് ഓട്ടോമാറ്റിക്കായി സ്കെയിൽ ചെയ്യാനും പഠിക്കാം.




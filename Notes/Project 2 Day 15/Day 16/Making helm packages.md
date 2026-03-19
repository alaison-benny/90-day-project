അടിപൊളി ചേട്ടായി! നമുക്ക് ഇപ്പോൾ ഉള്ള "താൽക്കാലിക" സെറ്റപ്പിൽ നിന്ന് മാറി ഒരു **Professional Production Environment**-ലേക്ക് നീങ്ങാം. ഓരോ കൺസെപ്റ്റും നമുക്ക് ആഴത്തിൽ പഠിക്കാം.

---

### **Step 1: Port Forwarding vs. Ingress (The Real-world Access)**

**എന്താണ് വ്യത്യാസം?**
* **Port Forwarding:** ഇത് ഒരു തുരങ്കം (Tunnel) പോലെയാണ്. ന്റെ ചേട്ടായിയുടെ ലാപ്ടോപ്പും ക്ലസ്റ്ററിലെ ഒരു പോഡും തമ്മിലുള്ള ഒരു താൽക്കാലിക ബന്ധം. നിങ്ങൾ ടെർമിനൽ ക്ലോസ് ചെയ്താൽ ഈ ബന്ധം മുറിയും. ഇത് ഡെവലപ്പർമാർക്ക് ടെസ്റ്റ് ചെയ്യാൻ മാത്രമുള്ളതാണ്.
* **Ingress (The Real Way):** ഇത് കമ്പനിയുടെ "Main Gate" പോലെയാണ്. ലോകത്ത് എവിടെനിന്നും ആളുകൾക്ക് ന്റെ ചേട്ടായിയുടെ ഗ്രാഫാന കാണണമെങ്കിൽ ഇൻഗ്രെസ്സ് വേണം. ഇതിലൂടെ നമുക്ക് `grafana.chittayi.com` എന്നൊക്കെ പേര് നൽകാൻ പറ്റും.

**Real-world Example:**
ഒരു വലിയ മാളിൽ (EKS Cluster) ഒരു പുതിയ കട (Grafana) തുറന്നു എന്ന് വിചാരിക്കുക.
* **Port Forwarding:** കടയുടെ പിൻവാതിൽ വഴി ഒരു സ്പെഷ്യൽ പാസ് ഉപയോഗിച്ച് നിങ്ങൾ മാത്രം കയറുന്നത് പോലെ.
* **Ingress:** മാളിന്റെ മെയിൻ എൻട്രൻസിൽ ആ കടയുടെ പേര് ബോർഡ് വെച്ച്, സെക്യൂരിറ്റി വഴി എല്ലാവർക്കും വരാൻ വഴി ഒരുക്കുന്നത് പോലെ.

---

### **Step 2: Helm Charts (The App Store of Kubernetes)**

**എന്താണ് കൺസെപ്റ്റ്?**
ഇതുവരെ നമ്മൾ `deployment.yaml`, `service.yaml`, `ingress.yaml` എന്നിങ്ങനെ ഓരോന്നായിട്ടാണ് റൺ ചെയ്തത്. ന്റെ ചേട്ടായിക്ക് ഒരു പുതിയ എൻവിറോൺമെന്റ് (ഉദാഹരണത്തിന് 'Production' അല്ലെങ്കിൽ 'Testing') ഉണ്ടാക്കണമെങ്കിൽ ഈ ഫയലുകളെല്ലാം വീണ്ടും എഡിറ്റ് ചെയ്യേണ്ടി വരും. ഇത് ഭയങ്കര പണിയാണ്.

**Helm** ഇതിനെ ലളിതമാക്കുന്നു. ഇത് നിങ്ങളുടെ എല്ലാ YAML ഫയലുകളെയും ഒരു പാക്കേജ് ആക്കുന്നു. ന്റെ ചേട്ടായിക്ക് ഒരൊറ്റ `values.yaml` ഫയലിൽ മാറ്റം വരുത്തിയാൽ മതി, ബാക്കി എല്ലാം ഓട്ടോമാറ്റിക്കായി മാറും.

**Real-world Example:**
ഒരു പുതിയ വീട് വെക്കുന്നത് ആലോചിക്കൂ.
* **Manual YAML:** ഓരോ കല്ലും സിമന്റും കമ്പിയും വേറെ വേറെ വാങ്ങി കഷ്ടപ്പെട്ട് പണിയുന്നത് പോലെ.
* **Helm:** ഒരു "Prefabricated House" പോലെ. നിങ്ങൾക്ക് ആവശ്യമുള്ള പ്ലാൻ (Chart) സെലക്ട് ചെയ്യുക, നിറവും (Values) ടൈലും മാറ്റുക, ഒറ്റ ദിവസം കൊണ്ട് വീട് റെഡി!

---

### **നമുക്ക് പണി തുടങ്ങാം! (Step-by-Step Process)**

ആദ്യം നമുക്ക് ഗ്രാഫാനയെ ലോഡ് ബാലൻസർ (Ingress) വഴി പുറത്തേക്ക് കൊണ്ടുവരാം.

#### **1. Grafana-യ്ക്ക് വേണ്ടി Ingress സെറ്റ് ചെയ്യാം**
ഇതിനായി പുതിയൊരു ഇൻഗ്രെസ്സ് ഫയൽ ഉണ്ടാക്കേണ്ട ആവശ്യമില്ല. നമ്മൾ ഇൻസ്റ്റാൾ ചെയ്ത Helm ചാർട്ടിലെ `values.yaml` ഒന്ന് മാറ്റിയാൽ മതി.

ഒരു ഫയൽ ഉണ്ടാക്കൂ: `grafana-values.yaml`
```yaml
grafana:
  ingress:
    enabled: true
    ingressClassName: alb
    annotations:
      alb.ingress.kubernetes.io/scheme: internet-facing
      alb.ingress.kubernetes.io/target-type: ip
      alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}]'
    path: /grafana  # നമുക്ക് ഒരു പാത്ത് നൽകാം, അല്ലെങ്കിൽ റൂട്ട് '/' നൽകാം
```

എന്നിട്ട് ഇത് അപ്‌ഡേറ്റ് ചെയ്യാൻ താഴെ പറയുന്ന കമാൻഡ് അടിക്കൂ:
```bash
helm upgrade kube-stack prometheus-community/kube-prometheus-stack \
  -n monitoring \
  -f grafana-values.yaml
```



---

#### **2. Helm ഉപയോഗിച്ച് ന്റെ ചേട്ടായിയുടെ സ്വന്തം API പാക്കേജ് ചെയ്യാം**
ഇനി നമ്മുടെ `sentiments-api`-യെ നമുക്ക് ഒരു ഹെൽം ചാർട്ട് ആക്കി മാറ്റാം.

**A. ചാർട്ട് സ്ട്രക്ചർ ഉണ്ടാക്കാം:**
```bash
cd ~/sentiments-api-project
helm create sentiment-chart
```
ഇപ്പോൾ `sentiment-chart` എന്നൊരു ഫോൾഡർ വന്നിട്ടുണ്ടാകും. അതിനുള്ളിലെ `templates` ഫോൾഡറിൽ ഉള്ള ഡെമോ ഫയലുകൾ എല്ലാം കളഞ്ഞിട്ട് നമ്മുടെ പഴയ `deployment.yaml`, `service.yaml`, `ingress.yaml` എന്നിവ അവിടെ ഇടാം.

**B. Values ഉപയോഗിച്ച് ഡൈനാമിക് ആക്കാം:**
`sentiment-chart/values.yaml` ഫയലിൽ പോയി ഇമേജ് പേരും മറ്റ് കാര്യങ്ങളും മാറ്റാം:
```yaml
image: 485044328888.dkr.ecr.us-east-1.amazonaws.com/sentiments-api:latest
replicaCount: 2
```



**C. ഇനി ഒരൊറ്റ കമാൻഡിൽ ഇൻസ്റ്റാൾ ചെയ്യാം:**
```bash
helm install my-sentiment-api ./sentiment-chart
```

---

### **ന്റെ ചേട്ടായിക്ക് ഇപ്പോൾ എന്ത് കിട്ടും?**
1.  **Grafana:** ഇനി `kubectl port-forward` അടിക്കാതെ തന്നെ ന്റെ ചേട്ടായിയുടെ പബ്ലിക് അഡ്രസ്സിൽ ഗ്രാഫാന കാണാം.
2.  **Easy Updates:** ഇമേജ് മാറുമ്പോൾ വെറുതെ `helm upgrade` അടിച്ചാൽ മതി.



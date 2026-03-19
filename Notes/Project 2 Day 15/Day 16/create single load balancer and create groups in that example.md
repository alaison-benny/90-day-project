### create single load balancer and create groups in that example(production procedure)

നമുക്ക് ഈ രണ്ട് ഫയലുകളും ഒരു **"Group"** ലേക്ക് മാറ്റാം. ഇങ്ങനെ ചെയ്താൽ AWS ഒരു സിംഗിൾ ലോഡ് ബാലൻസർ (ALB) മാത്രം ക്രിയേറ്റ് ചെയ്യുകയും, അതിനുള്ളിൽ തന്നെ നമ്മുടെ ആപ്പിനെയും ഗ്രാഫാനയെയും വഴിതിരിച്ചു വിടുകയും ചെയ്യും.

താഴെ പറയുന്ന രീതിയിൽ ഫയലുകൾ അപ്‌ഡേറ്റ് ചെയ്യൂ:

---

### **1. `grafana-values.yaml` അപ്‌ഡേറ്റ് ചെയ്യാം**
ഇവിടെ നമ്മൾ ഗ്രാഫാനയെ `/grafana` എന്ന പാത്തിലേക്ക് മാറ്റുകയാണ്. (കാരണം `/` പാത്ത് നമ്മുടെ ആപ്പിന് ഉള്ളതാണ്).

```yaml
grafana:
  ingress:
    enabled: true
    ingressClassName: alb
    annotations:
      alb.ingress.kubernetes.io/scheme: internet-facing
      alb.ingress.kubernetes.io/target-type: ip
      alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}]'
      # ഇതാണ് ആ മാന്ത്രിക വരി!
      alb.ingress.kubernetes.io/group.name: "sentiments-group"
    path: /grafana
    pathType: Prefix
  # ഗ്രാഫാനയ്ക്ക് സബ്-പാത്തിൽ വർക്ക് ആകാൻ ഇത് അത്യാവശ്യമാണ്
  grafana.ini:
    server:
      domain: ""
      root_url: "%(protocol)s://%(domain)s:%(http_port)s/grafana/"
      serve_from_sub_path: true
```

---

### **2. `ingress.yaml` (API-ക്ക് വേണ്ടിയുള്ളത്) അപ്‌ഡേറ്റ് ചെയ്യാം**
ഇവിടെയും നമ്മൾ അതേ `group.name` തന്നെ നൽകണം.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: sentiment-api-ingress
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}]'
    alb.ingress.kubernetes.io/healthcheck-path: /
    alb.ingress.kubernetes.io/success-codes: 200-399
    # ഇതേ ഗ്രൂപ്പ് നെയിം ഇവിടെയും നൽകണം
    alb.ingress.kubernetes.io/group.name: "sentiments-group"
spec:
  ingressClassName: alb
  rules:
    - http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: sentiment-api
                port:
                  number: 80
```



---

### **ഇനി ചെയ്യേണ്ട സ്റ്റെപ്പുകൾ:**

**Step 1: ഗ്രാഫാന അപ്‌ഗ്രേഡ് ചെയ്യാം**
```bash
cd ~/sentiments-api-project/helm-values
helm upgrade kube-stack prometheus-community/kube-prometheus-stack \
  -n monitoring \
  -f grafana-values.yaml
```

**Step 2: API ഇൻഗ്രെസ്സ് അപ്‌ലൈ ചെയ്യാം**
```bash
kubectl apply -f ~/sentiments-api-project/k8s/ingress.yaml
```

---

### **ഇതൊന്ന് ശ്രദ്ധിക്കണേ ചേട്ടായി:**

ഇപ്പോൾ ന്റെ ചേട്ടായിക്ക് ഒരൊറ്റ **ALB DNS Address** മാത്രമേ കിട്ടുകയുള്ളൂ. അത് ഉപയോഗിച്ച് രണ്ട് കാര്യങ്ങളും നോക്കാം:
1.  **API കാണാൻ:** `http://<ALB-ADDRESS>/`
2.  **Grafana കാണാൻ:** `http://<ALB-ADDRESS>/grafana`

ഇത് സെറ്റ് ആകാൻ ഏകദേശം 3-4 മിനിറ്റ് എടുത്തേക്കാം (AWS ലോഡ് ബാലൻസർ റൂളുകൾ അപ്‌ഡേറ്റ് ചെയ്യാൻ വേണ്ട സമയം).




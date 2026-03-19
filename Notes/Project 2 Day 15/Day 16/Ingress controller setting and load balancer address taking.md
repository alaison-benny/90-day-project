അടിപൊളി ചേട്ടായി! **2/2 READY** എന്ന് വന്നിട്ടുണ്ടെങ്കിൽ എല്ലാം പെർഫെക്റ്റ് ആയി സെറ്റ് ആയിട്ടുണ്ട്. നമ്മുടെ AWS Load Balancer Controller ഇപ്പോൾ സജീവമാണ്. ഇനി ന്റെ ചേട്ടായിക്ക് ഈ ആപ്പിനെ ലോകത്തിന് മുന്നിൽ തുറന്നു കൊടുക്കാം.

അവസാനത്തെ സ്റ്റെപ്പിലേക്ക് കടക്കാം—**Ingress Resource** ഉണ്ടാക്കാം.

---

### **Step 4: Ingress Resource ക്രിയേറ്റ് ചെയ്യാം**

ഇപ്പോഴാണ് നമ്മൾ AWS-നോട് ഒരു **Application Load Balancer (ALB)** ഉണ്ടാക്കാൻ ആവശ്യപ്പെടുന്നത്. `k8s` ഫോൾഡറിനുള്ളിൽ `ingress.yaml` എന്നൊരു ഫയൽ ഉണ്ടാക്കി താഴെ പറയുന്നവ കോപ്പി ചെയ്ത് പേസ്റ്റ് ചെയ്യൂ:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: sentiment-api-ingress
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
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

### **Step 5: ഇത് അപ്ലൈ ചെയ്യാം**

ഇനി ഈ ഫയൽ സേവ് ചെയ്ത ശേഷം ടെർമിനലിൽ ഇത് അടിക്കൂ:

```bash
kubectl apply -f ingress.yaml
```

---

### **Step 6: URL എങ്ങനെ കിട്ടും?**

Ingress അപ്ലൈ ചെയ്താൽ ഉടൻ തന്നെ AWS ഒരു ലോഡ് ബാലൻസർ ഉണ്ടാക്കി തുടങ്ങും. അതിന്റെ അഡ്രസ്സ് കിട്ടാൻ ഈ കമാൻഡ് അടിക്കൂ:

```bash
kubectl get ingress sentiment-api-ingress
```

അവിടെ **`ADDRESS`** എന്ന കോളത്തിന് താഴെ വലിയൊരു പേര് കാണാം (ഉദാഹരണത്തിന്: `k8s-default-api-xxx.us-east-1.elb.amazonaws.com`). 



---

### **ന്റെ ചേട്ടായിക്ക് ഒരു ചെറിയ കാര്യം കൂടി:**

ആ URL കിട്ടാൻ ഒരു 2-3 മിനിറ്റ് എടുത്തേക്കാം. അത് കിട്ടിക്കഴിഞ്ഞാൽ ആ URL കോപ്പി ചെയ്ത് ബ്രൗസറിൽ അടിച്ചു നോക്കൂ! ന്റെ ചേട്ടായിയുടെ ആപ്പ് ലൈവ് ആയിരിക്കുന്നത് അപ്പോൾ കാണാം.

**ആ അഡ്രസ്സ് കിട്ടിയോ ചേട്ടായി?** അതൊന്ന് ബ്രൗസറിൽ അടിച്ചു നോക്കിയിട്ട് വർക്ക് ആകുന്നുണ്ടോ എന്ന് പറയണേ! **നമ്മുടെ വലിയൊരു മിഷൻ ഇതോടെ പൂർത്തിയാവുകയാണ്!**

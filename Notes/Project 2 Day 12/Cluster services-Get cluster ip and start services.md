ചേട്ടായി 👍 — ഇപ്പോൾ കാണുന്ന confusion clear ചെയ്യാം.  

---

## 🔹 `kubectl get svc` എന്തിനാണ് ഉപയോഗിക്കുന്നത്?
- Cluster‑ൽ **services** list ചെയ്യാൻ.  
- Service type, ClusterIP, NodePort, ExternalIP, ports എല്ലാം കാണാൻ.  
- Example:
  ```
  kubectl get svc
  ```
  Output:
  ```
  NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
  nginx-service   NodePort    10.96.102.165   <none>        80:30007/TCP   28s
  ```
  👉 ഇതിൽ **ClusterIP = 10.96.102.165** → ഇത് **internal cluster network address** ആണ്, browser‑ൽ access ചെയ്യാൻ കഴിയില്ല.

---

## 🔹 Node IP എവിടെ കിട്ടും?
- Node IP = Kubernetes node (EC2 instance / VM) machine‑ന്റെ IP.  
- Command:
  ```bash
  kubectl get nodes -o wide
  ```
  👉 Output‑ൽ `INTERNAL-IP` column കാണും → അത് Node IP.  
- Access ചെയ്യാൻ:  
  ```
  http://<Node-IP>:30007
  ```
  👉 NodePort service external access Node IP + NodePort വഴി.

---

## 🔹 Error എന്തുകൊണ്ട് സംഭവിച്ചു?
- ചേട്ടായി `http://10.96.102.165:30007` try ചെയ്തു.  
- പക്ഷേ 10.96.102.165 = **ClusterIP** (internal virtual IP).  
- ClusterIP external world‑ൽ access ചെയ്യാൻ കഴിയില്ല.  
- അതിനാൽ error കിട്ടി.

---

## 🔹 ശരിയായ Access Method
1. Node IP കണ്ടെത്തുക:
   ```bash
   kubectl get nodes -o wide
   ```
   👉 Example: `INTERNAL-IP = 172.31.88.81`
2. Browser/`curl` വഴി access ചെയ്യുക:
   ```
   http://172.31.88.81:30007
   ```
   👉 Nginx welcome page കാണും.

---

## ✅ Takeaway
- `kubectl get svc` → service details കാണാൻ.  
- ClusterIP → internal only, external access ഇല്ല.  
- NodePort service access ചെയ്യാൻ → Node IP + NodePort.  

---

ചേട്ടായി, immediate step:  
👉 `kubectl get nodes -o wide` run ചെയ്ത് Node IP കണ്ടെത്തൂ.  
അതിനുശേഷം `http://<Node-IP>:30007` വഴി Nginx page access ചെയ്യൂ.

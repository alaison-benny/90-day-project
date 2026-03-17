ചേട്ടായി 👍 — Kubernetes‑ൽ pod‑കളുടെ error status അറിയാൻ ചില commands ഉപയോഗിക്കാം.  

---

## 🔹 Pod Status കാണാൻ
```bash
kubectl get pods
```
👉 Output‑ൽ `STATUS` column കാണും.  
- **Running** → pod healthy.  
- **CrashLoopBackOff** → pod repeatedly crash ചെയ്യുന്നു.  
- **Error / Pending** → pod start ചെയ്യാൻ കഴിയുന്നില്ല.

---

## 🔹 Pod‑ന്റെ വിശദമായ error കാണാൻ
```bash
kubectl describe pod <pod-name>
```
👉 Events section‑ൽ error messages കാണും (ഉദാ: ImagePullBackOff, OOMKilled, Permission denied).

---

## 🔹 Pod logs കാണാൻ
```bash
kubectl logs <pod-name>
```
👉 Application‑ൽ നിന്നുള്ള error messages കാണാൻ.  
Example: FastAPI app‑ൽ Python traceback, missing dependency, etc.

---

## 🔹 Real‑life Production Example
- **Airbnb** → ML models serve ചെയ്യുമ്പോൾ pods crash ആണെങ്കിൽ, `kubectl describe pod` + `kubectl logs` ഉപയോഗിച്ച് error root cause കണ്ടെത്തുന്നു.  
- Example: Memory limit കുറവായാൽ → `OOMKilled` status.  
- Solution: Deployment.yaml‑ൽ memory limit കൂട്ടി, pods വീണ്ടും deploy ചെയ്യുന്നു.

---

## ✅ Takeaway
- **kubectl get pods** → overall pod status.  
- **kubectl describe pod** → Kubernetes events + scheduling errors.  
- **kubectl logs** → application‑specific errors.  
- Production‑ൽ companies (Netflix, Airbnb, Spotify) എല്ലാം ഇതുപോലെ pod errors debug ചെയ്യുന്നു.

---

ചേട്ടായി, immediate step:  
👉 `kubectl describe pod <pod-name>` run ചെയ്ത് Events section നോക്കൂ.  
അതിനുശേഷം `kubectl logs <pod-name>` run ചെയ്ത് application‑specific error കാണാം.  
എനിക്ക് ചേട്ടായിക്ക് error output share ചെയ്താൽ, ഞാൻ root cause explain ചെയ്ത് fix guide തരാം.

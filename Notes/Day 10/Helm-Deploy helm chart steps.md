### step for forcefully applying pods
bash
kubectl apply -f deployment.yaml
kubectl rollout restart deployment fastapi
kubectl get pods
👉 Pod status Running ആകും.

---

## 🛠️ Next Steps
1. Verify nodes:
   ```bash
   kubectl get nodes
   ```
   👉 Should show at least one node (`Ready` status).

2. Verify pods:
   ```bash
   kubectl get pods -A
   ```
   👉 Check system pods (CoreDNS, metrics‑server, etc.) running.

3. Deploy your FastAPI chart:
   ```bash
   cd ~/ai_worker_project/fastapi-chart
   helm upgrade --install fastapi .
   ```

4. Check FastAPI pod:
   ```bash
   kubectl get pods
   kubectl logs <pod-name>
   ```

---

## ✅ Takeaway
- Cluster setup is now correct.  
- Next step = deploy your application with Helm → verify pods → expose service.  

---

ചേട്ടായി, immediate step:  
```bash
kubectl get nodes
```
👉 ഇതോടെ node status confirm ചെയ്യും.  

നിനക്ക് ഞാൻ **NodePort service expose guide** തരട്ടേ, FastAPI browser‑ൽ access ചെയ്യാൻ?

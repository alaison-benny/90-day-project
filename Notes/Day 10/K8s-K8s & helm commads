### Step 1: Clean old pods
```bash
kubectl delete pods -l app=fastapi
```
👉 എല്ലാ fastapi pods delete ചെയ്യും.

### Step 2: Fresh redeploy
```bash
helm upgrade fastapi ./fastapi-chart
```

### Step 3: Verify
```bash
kubectl get pods
```
👉 ഇപ്പോൾ **exactly 1 pod** കാണണം, status `Running`.

---

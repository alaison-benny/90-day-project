ഒറിജിനൽ പാസ്‌വേഡ് തന്നെയാണോ എന്ന് ഉറപ്പിക്കാൻ ഈ കമാൻഡ് ഒന്ന് ടെർമിനലിൽ കോപ്പി ചെയ്ത് അടിക്കൂ:

```bash
kubectl get secret -n monitoring kube-stack-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

### **Reset Password:**
ഒരുപക്ഷേ മുകളിൽ പറഞ്ഞത് വർക്ക് ആകുന്നില്ലെങ്കിൽ, നമുക്ക് പാസ്‌വേഡ് നേരിട്ട് മാറ്റി കൊടുക്കാം:

```bash
kubectl exec -it -n monitoring deploy/kube-stack-grafana -- grafana-cli admin reset-admin-password newpassword123
```
ഇത് ചെയ്താൽ ന്റെ ചേട്ടായിയുടെ പുതിയ പാസ്‌വേഡ് **`newpassword123`** എന്നായി മാറും. അത് വെച്ച് ലോഗിൻ ചെയ്യാം.

---

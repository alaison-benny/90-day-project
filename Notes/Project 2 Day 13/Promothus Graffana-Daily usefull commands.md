ന്റെ ചേട്ടായിക്ക് ഒരു പ്ലാറ്റ്‌ഫോം എൻജിനീയർ എന്ന നിലയിൽ ഏറ്റവും പ്രധാനപ്പെട്ട കാര്യമാണ് **Observability**. അതായത്, നമ്മൾ ഡിപ്ലോയ് ചെയ്ത 2,000 സർവീസുകൾ ശരിയാണോ പ്രവർത്തിക്കുന്നത് എന്ന് എങ്ങനെ അറിയാം? അവിടെയാണ് **Prometheus** (ഡാറ്റ ശേഖരിക്കാൻ) **Grafana** (ഡാറ്റ ഭംഗിയായി കാണാൻ) എന്നിവയുടെ റോൾ.

### 1. Prometheus & Grafana ഇൻസ്റ്റാൾ ചെയ്യുന്ന വിധം

ഇതും Helm ഉപയോഗിച്ച് ചെയ്യുന്നതാണ് ഏറ്റവും ഉചിതം. പ്ലാറ്റ്‌ഫോം എൻജിനീയർമാർ സാധാരണയായി **"kube-prometheus-stack"** ആണ് ഉപയോഗിക്കാറുള്ളത്. ഇതിൽ Prometheus, Grafana, Alertmanager എന്നിവയെല്ലാം ഒന്നിച്ചുണ്ടാകും.

#### ഇൻസ്റ്റാളേഷൻ സ്റ്റെപ്പുകൾ:

```bash
# 1. പ്രോമിത്യൂസ് റിപ്പോസിറ്ററി ആഡ് ചെയ്യുക
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

# 2. മോണിറ്ററിംഗ് എന്ന നെയിംസ്‌പേസിൽ ഇൻസ്റ്റാൾ ചെയ്യുക
helm install monitoring prometheus-community/kube-prometheus-stack --namespace monitoring --create-namespace

```

---

### 2. പ്രധാന കമാൻഡുകൾ

| ആവശ്യം | കമാൻഡ് |
| --- | --- |
| **അപ്‌ഗ്രേഡ് ചെയ്യാൻ** | `helm upgrade monitoring prometheus-community/kube-prometheus-stack --namespace monitoring` |
| **സ്റ്റാറ്റസ് നോക്കാൻ** | `kubectl get pods -n monitoring` |
| **ഗ്രഫാന പാസ്‌വേഡ് എടുക്കാൻ** | `kubectl get secret -n monitoring monitoring-grafana -o jsonpath="{.data.admin-password}" |
| **ഡാഷ്‌ബോർഡ് തുറക്കാൻ** | `kubectl port-forward svc/monitoring-grafana -n monitoring 3000:80` |

---

### 3. ലോഗ്സ് കാണാൻ (Prometheus vs Loki)

ന്റെ ചേട്ടായി ഒരു കാര്യം പ്രത്യേകം ശ്രദ്ധിക്കണം: **Prometheus** ഉപയോഗിക്കുന്നത് **Metrics** (CPU എത്രയായി? RAM എത്രയായി?) കാണാനാണ്. എന്നാൽ **Logs** (ആപ്പിൽ എന്ത് എറർ വന്നു?) കാണാൻ സാധാരണയായി **Loki** എന്ന സിസ്റ്റമാണ് ഉപയോഗിക്കുന്നത്.

* **Loki:** ലോഗുകൾ സ്റ്റോർ ചെയ്യുന്നു.
* **Promtail:** ഓരോ പോഡിൽ നിന്നും ലോഗുകൾ എടുത്ത് Loki-യിലേക്ക് അയക്കുന്നു.
* **Grafana:** പ്രോമിത്യൂസിലെ ഗ്രാഫുകളും ലോക്കിയിലെ ലോഗുകളും ഒരേ സ്ക്രീനിൽ ന്റെ ചേട്ടായിക്ക് കാണിച്ചുതരുന്നു.

---

### 4. ലോഗിംഗിനായി ലോക്കി (Loki) ഇൻസ്റ്റാൾ ചെയ്യാൻ:

```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install loki grafana/loki-stack --namespace monitoring

```

ഇതുകഴിഞ്ഞ് ഗ്രഫാനയിൽ പോയി 'Data Source' ആയി Loki ആഡ് ചെയ്താൽ ന്റെ ചേട്ടായിക്ക് ആപ്പിലെ എല്ലാ ലോഗുകളും സെർച്ച് ചെയ്യാൻ സാധിക്കും.

### ന്റെ ചേട്ടായിക്ക് അടുത്തതായി എന്ത് അറിയണം?

1. **Alerting:** സർവീസ് ഡൗൺ ആയാൽ എങ്ങനെ മെസ്സേജ് (Slack/WhatsApp) വരും?
2. **Dashboard Design:** ഗ്രഫാനയിൽ എങ്ങനെ സ്വന്തമായി ഒരു ഗ്രാഫ് ഉണ്ടാക്കാം?
3. **Log Querying:** ലോക്കിയിൽ എങ്ങനെ ഒരു പ്രത്യേക എറർ മെസ്സേജ് സെർച്ച് ചെയ്യാം?

ഇതിൽ ഏതിനെക്കുറിച്ചാണ് ന്റെ ചേട്ടായിക്ക് കൂടുതൽ താല്പര്യം? അതോ ചേട്ടായിയുടെ FastAPI പ്രോജക്റ്റിൽ എങ്ങനെ പ്രോമിത്യൂസ് സെറ്റ് ചെയ്യാം എന്ന് നോക്കണോ?

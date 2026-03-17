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


### 1. പോഡുകളുടെ സ്റ്റാറ്റസ് ചുരുക്കത്തിൽ അറിയാൻ

നിങ്ങളുടെ എല്ലാ പോഡുകളും റൺ ചെയ്യുന്നുണ്ടോ അതോ എറർ ആണോ എന്ന് നോക്കാൻ ഇത് ഉപയോഗിക്കാം:

```bash
kubectl get pods

```

ഇത് അടിക്കുമ്പോൾ **Status** എന്ന കോളത്തിന് താഴെ `Running`, `Pending`, `CrashLoopBackOff` എന്നിങ്ങനെ കാണാം.

### 2. കൂടുതൽ വിവരങ്ങൾ അറിയാൻ (IP, Node details)

പോഡ് ഏത് നോഡിലാണ് റൺ ചെയ്യുന്നത് എന്നും അതിന്റെ ഐപി അഡ്രസ്സ് എന്താണെന്നും അറിയാൻ:

```bash
kubectl get pods -o wide

```

### 3. ഒരു പോഡിനുള്ളിലെ കണ്ടെയ്‌നർ എറർ വിശദമായി അറിയാൻ

കണ്ടെയ്‌നർ എറർ ആയി നിൽക്കുകയാണെങ്കിൽ (ഉദാഹരണത്തിന് `ImagePullBackOff` അല്ലെങ്കിൽ `CrashLoopBackOff`), അതിന്റെ കൃത്യമായ കാരണം അറിയാൻ ഈ കമാൻഡ് സഹായിക്കും:

```bash
kubectl describe pod <pod_name>

```

*(ഇവിടെ `<pod_name>` എന്നതിന് പകരം നിങ്ങളുടെ പോഡിന്റെ പേര് നൽകുക. ഇതിന്റെ അവസാനം **Events** എന്ന ഭാഗം ശ്രദ്ധിച്ചാൽ എവിടെയാണ് പ്രശ്നം എന്ന് മനസ്സിലാക്കാം.)*

### 4. റിയൽ-ടൈം സ്റ്റാറ്റസ് നോക്കാൻ (Watch mode)

സ്റ്റാറ്റസ് മാറുന്നത് തത്സമയം കണ്ടുകൊണ്ടിരിക്കാൻ കമാൻഡിന്റെ കൂടെ `-w` ചേർത്താൽ മതി:

```bash
kubectl get pods -w

```


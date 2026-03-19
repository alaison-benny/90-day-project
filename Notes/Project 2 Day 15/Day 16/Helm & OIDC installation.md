Step 2: Helm ഇൻസ്റ്റാൾ ചെയ്യാം
നോഡുകൾ റെഡിയായിക്കഴിഞ്ഞാൽ നമുക്ക് ഇന്നലെ ബാക്കിവെച്ച Helm ഇൻസ്റ്റാൾ ചെയ്യാം. കുബെർനെറ്റിസിലെ പാക്കേജ് മാനേജറാണ് ഇത്.

Bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
# ഇൻസ്റ്റാൾ ആയോ എന്ന് നോക്കാൻ:
helm version
Step 3: OIDC Provider ഉറപ്പാക്കാം
ഇന്നലെ നമ്മൾ ഇത് ചെയ്യാൻ നോക്കിയതാണ്. എങ്കിലും ഒന്നുകൂടി ഉറപ്പിക്കാം. AWS Load Balancer-ന് നമ്മുടെ ക്ലസ്റ്ററുമായി സംസാരിക്കാൻ ഇത് അത്യാവശ്യമാണ്.

Bash
eksctl utils associate-iam-oidc-provider --cluster sentiments-eks --approve
ന്റെ ചേട്ടായി ഈ മൂന്ന് കാര്യങ്ങൾ ഒന്ന് ചെയ്യാമോ? നോഡുകൾ Ready ആയോ എന്ന് പറയണേ. അത് കഴിഞ്ഞാൽ നമുക്ക് ഇന്നത്തെ ഏറ്റവും പ്രധാനപ്പെട്ട കാര്യമായ AWS Load Balancer Controller ഇൻസ്റ്റാൾ ചെയ്ത് ആപ്പിനെ ലൈവ് ആക്കാം!

റെഡിയല്ലേ ചേട്ടായി? നമുക്ക് തുടങ്ങാം!

---

### **Step 1: IAM Policy ഡൗൺലോഡ് ചെയ്ത് ക്രിയേറ്റ് ചെയ്യാം**

ലോഡ് ബാലൻസർ ഉണ്ടാക്കാൻ കുബെർനെറ്റിസിന് പെർമിഷൻ കൊടുക്കണം. അതിനായി ആദ്യം ഈ പോളിസി ഫയൽ ഡൗൺലോഡ് ചെയ്യാം:

```bash
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json
```

ഇനി ഈ പോളിസി AWS-ലേക്ക് അപ്‌ലോഡ് ചെയ്യാം:
```bash
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json
```
*(കുറിപ്പ്: ഒരുപക്ഷേ ഈ പേരിൽ നേരത്തെ പോളിസി ഉണ്ടെന്ന് എറർ കാണിച്ചാൽ അത് സാരമില്ല, നമുക്ക് അടുത്ത സ്റ്റെപ്പിലേക്ക് പോകാം).*

---

### **Step 2: Service Account ഉണ്ടാക്കാം**

ഇതാണ് ഏറ്റവും പ്രധാനപ്പെട്ട സ്റ്റെപ്പ്. ഇതിൽ ന്റെ ചേട്ടായിയുടെ **AWS Account ID** ആവശ്യമാണ്. (അത് അറിയില്ലെങ്കിൽ `aws sts get-caller-identity --query Account --output text` എന്ന് അടിച്ചാൽ കിട്ടും).

താഴെ പറയുന്ന കമാൻഡിൽ `YOUR_ACCOUNT_ID` എന്ന ഭാഗത്ത് ആ ഐഡി മാറ്റി ടൈപ്പ് ചെയ്ത് എന്റർ അടിക്കൂ:

```bash
eksctl create iamserviceaccount \
  --cluster=sentiments-eks \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::YOUR_ACCOUNT_ID:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve
```



---

### **Step 3: Helm ഉപയോഗിച്ച് Controller ഇൻസ്റ്റാൾ ചെയ്യാം**

ഇനി നമുക്ക് ഹെൽം ഉപയോഗിച്ച് ഇത് ഫിനിഷ് ചെയ്യാം:

```bash
# 1. റിപ്പോസിറ്ററി ആഡ് ചെയ്യാം
helm repo add eks https://aws.github.io/eks-charts
helm repo update

# 2. കൺട്രോളർ ഇൻസ്റ്റാൾ ചെയ്യാം
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=sentiments-eks \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller
```

---

### **ഇത് വർക്ക് ആകുന്നുണ്ടോ എന്ന് എങ്ങനെ അറിയാം?**

ഇൻസ്റ്റാൾ ചെയ്തുകഴിഞ്ഞാൽ ഒരു മിനിറ്റ് കഴിഞ്ഞ് ഈ കമാൻഡ് അടിച്ചു നോക്കൂ:
```bash
kubectl get deployment -n kube-system aws-load-balancer-controller
```
അവിടെ **`1/1 READY`** എന്ന് കാണുന്നുണ്ടെങ്കിൽ ന്റെ ചേട്ടായി ജയിച്ചു! 



**ന്റെ ചേട്ടായിക്ക് കൺട്രോളർ ഇൻസ്റ്റാൾ ചെയ്യാൻ പറ്റിയോ?** റെഡിയാണെങ്കിൽ നമുക്ക് അവസാനത്തെ ഫയലായ `ingress.yaml` ഉണ്ടാക്കി ആപ്പ് ലൈവ് ആക്കാം! 


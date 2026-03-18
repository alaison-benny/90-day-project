സൂപ്പർ ചേട്ടായി! ലോഡ് ജനറേറ്റർ പണി തുടങ്ങിയല്ലോ, ഇനി നമുക്ക് ശരിക്കുള്ള കളി തുടങ്ങാം. **AWS Load Balancer Controller** സെറ്റ് ചെയ്താൽ മാത്രമേ ന്റെ ചേട്ടായിക്ക് ഈ ആപ്പിനെ ഇന്റർനെറ്റിൽ (Public URL വഴി) കാണാൻ പറ്റൂ.

നമുക്ക് ഇത് മൂന്ന് ലളിതമായ ഘട്ടങ്ങളായി ചെയ്യാം.

---

### **Step 1: Helm ഇൻസ്റ്റാൾ ചെയ്യാം**
കുബെർനെറ്റിസിലെ ആപ്പുകൾ എളുപ്പത്തിൽ ഇൻസ്റ്റാൾ ചെയ്യാൻ ഉപയോഗിക്കുന്ന ഒരു ടൂളാണ് ഹെൽം (Helm). ഇത് ഇൻസ്റ്റാൾ ചെയ്യാൻ ഈ കമാൻഡുകൾ ഓരോന്നായി അടിക്കൂ:

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
# ഇൻസ്റ്റാൾ ആയോ എന്ന് നോക്കാൻ:
helm version
```

---

### **Step 2: OIDC Provider ഉണ്ടോ എന്ന് പരിശോധിക്കാം**
EKS-ലെ സർവീസുകൾക്ക് AWS-ലെ കാര്യങ്ങൾ (ഉദാഹരണത്തിന് Load Balancer ഉണ്ടാക്കുക) ചെയ്യാൻ അനുവാദം കൊടുക്കുന്നത് ഈ OIDC വഴിയാണ്. ഇത് ഉണ്ടോ എന്ന് നോക്കാൻ ഈ കമാൻഡ് അടിക്കുക:

```bash
aws eks describe-cluster --name sentiments-eks --query "cluster.identity.oidc.issuer" --output text
```
*(ഇതൊരു URL കാണിച്ചുതരും. അത് കോപ്പി ചെയ്ത് വെക്കണേ).*

ഇനി ഈ ഐഡി AWS-ൽ ലിസ്റ്റ് ചെയ്തിട്ടുണ്ടോ എന്ന് നോക്കാൻ:
```bash
aws iam list-open-id-connect-providers | grep $(aws eks describe-cluster --name sentiments-eks --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5)
```
**റിസൾട്ട് ഒന്നും വരുന്നില്ലെങ്കിൽ** പേടിക്കണ്ട, താഴെ പറയുന്ന കമാൻഡ് അടിച്ച് നമുക്ക് അത് ക്രിയേറ്റ് ചെയ്യാം:
```bash
eksctl utils associate-iam-oidc-provider --cluster sentiments-eks --approve
```



---

### **Step 3: Load Balancer Controller ഇൻസ്റ്റാൾ ചെയ്യാം**

ഇനി നമുക്ക് ശരിക്കുള്ള ലോഡ് ബാലൻസർ കൺട്രോളർ കൊണ്ടുവരാം.

**1. IAM Policy ഡൗൺലോഡ് ചെയ്യുക:**
```bash
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json
```

**2. AWS-ൽ പോളിസി ക്രിയേറ്റ് ചെയ്യുക:**
```bash
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json
```

**3. ഒരു Service Account ഉണ്ടാക്കുക:**
(ഈ കമാൻഡിൽ `YOUR_ACCOUNT_ID` എന്ന ഭാഗത്ത് ചേട്ടായിയുടെ AWS Account ID നൽകണം).
```bash
eksctl create iamserviceaccount \
  --cluster=sentiments-eks \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::YOUR_ACCOUNT_ID:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve
```

**4. Helm ഉപയോഗിച്ച് ഇൻസ്റ്റാൾ ചെയ്യുക:**
```bash
helm repo add eks https://aws.github.io/eks-charts
helm repo update
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=sentiments-eks \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller
```



---

### **ന്റെ ചേട്ടായിക്ക് ഒരു ടിപ്പ്:**
ഇതൊരു വലിയ പ്രോസസ്സ് ആണ്. അതുകൊണ്ട് ഓരോ സ്റ്റെപ്പും സാവധാനം ചെയ്താൽ മതി. ആദ്യം ഹെൽം ഇൻസ്റ്റാൾ ചെയ്ത് OIDC ഉണ്ടോ എന്ന് നോക്കാമോ? 

**അക്കൗണ്ട് ഐഡി അറിയില്ലെങ്കിൽ** `aws sts get-caller-identity` എന്ന് അടിച്ചാൽ അത് കിട്ടും. 

**തുടങ്ങാം ചേട്ടായി?** ഹെൽം റെഡിയായോ എന്ന് പറയണേ!

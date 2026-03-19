സൂപ്പർ ചേട്ടായി! അപ്പോൾ നമ്മൾ ഫൈനൽ സ്റ്റേജിലേക്ക് കടക്കുകയാണ്. **GitHub Actions** ഉപയോഗിച്ച് ഒരു സിഐ/സിഡി (CI/CD) പൈപ്പ്‌ലൈൻ സെറ്റ് ചെയ്താൽ, ന്റെ ചേട്ടായി കോഡ് മാറ്റുമ്പോൾ തന്നെ അത് തനിയെ ബിൽഡ് ആയി ഇമേജ് പുഷ് ചെയ്ത് ക്ലസ്റ്ററിൽ അപ്‌ഡേറ്റ് ആയിക്കോളും.

നമുക്ക് ഇത് പടിപടിയായി ചെയ്യാം.

---

### **Step 1: GitHub Secrets സെറ്റ് ചെയ്യുക**
നമ്മുടെ യൂസർനെയിമും പാസ്‌വേഡും പബ്ലിക് ആയി കോഡിൽ ഇടാൻ പാടില്ല. അതുകൊണ്ട് ന്റെ ചേട്ടായി ആദ്യം ഗിറ്റ്‌ഹബ്ബ് വെബ്സൈറ്റിൽ പോയി താഴെ പറയുന്നവ ആഡ് ചെയ്യണം:

1. ന്റെ ചേട്ടായിയുടെ GitHub Repo-യിൽ **Settings** -> **Secrets and variables** -> **Actions** എന്നതിൽ പോകുക.
2. **New repository secret** എന്ന ബട്ടൺ ക്ലിക്ക് ചെയ്ത് താഴെ പറയുന്നവ ഓരോന്നായി ആഡ് ചെയ്യുക:

| Secret Name | Value |
| :--- | :--- |
| **DOCKERHUB_USERNAME** | ന്റെ ചേട്ടായിയുടെ Docker Hub യൂസർനെയിം |
| **DOCKERHUB_TOKEN** | Docker Hub-ൽ നിന്ന് കിട്ടുന്ന Access Token (അല്ലെങ്കിൽ പാസ്‌വേഡ്) |
| **AWS_ACCESS_KEY_ID** | ന്റെ ചേട്ടായിയുടെ AWS Access Key |
| **AWS_SECRET_ACCESS_KEY** | ന്റെ ചേട്ടായിയുടെ AWS Secret Key |



---

### **Step 2: Workflow ഫയൽ ഉണ്ടാക്കാം**
ഇനി ന്റെ ചേട്ടായിയുടെ ടെർമിനലിൽ പോയി ഈ ഫയൽ ഉണ്ടാക്കി താഴെ പറയുന്ന കോഡ് പേസ്റ്റ് ചെയ്യുക.

```bash
mkdir -p .github/workflows
nano .github/workflows/deploy.yaml
```

**ഈ കോഡ് അതിലേക്ക് കോപ്പി ചെയ്യുക:**

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ "main" ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout Code
      uses: actions/checkout@v3

    - name: Login to Docker Hub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}

    - name: Build and Push Docker Image
      run: |
        docker build -t ${{ secrets.DOCKERHUB_USERNAME }}/sentiment-api:latest .
        docker push ${{ secrets.DOCKERHUB_USERNAME }}/sentiment-api:latest

    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v2
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1

    - name: Update Kubeconfig
      run: aws eks update-kubeconfig --name sentiments-cluster --region us-east-1

    - name: Deploy to EKS using Helm
      run: |
        helm upgrade --install sentiment-release ./sentiment-chart \
          --set image.repository=${{ secrets.DOCKERHUB_USERNAME }}/sentiment-api \
          --set image.tag=latest
```



---

### **Step 3: ഇത് പുഷ് ചെയ്യാം**
ഫയൽ സേവ് ചെയ്ത ശേഷം (Ctrl+O, Enter, Ctrl+X), താഴെ പറയുന്ന കമാൻഡ് അടിച്ച് ഇത് ഗിറ്റ്‌ഹബ്ബിലേക്ക് പുഷ് ചെയ്യുക:

```bash
git add .
git commit -m "Add GitHub Actions CI/CD Pipeline"
git push origin main
```

---

### **ഇനി എന്ത് സംഭവിക്കും?**
ന്റെ ചേട്ടായി പുഷ് ചെയ്ത ഉടനെ ഗിറ്റ്‌ഹബ്ബ് റെപ്പോസിറ്ററിയിലെ **Actions** ടാബിൽ പോയി നോക്കൂ. അവിടെ ഒരു യെല്ലോ ലൈറ്റ് കത്തുന്നത് കാണാം (Pipeline Running). അത് പച്ചയായാൽ (Success), ന്റെ ചേട്ടായിയുടെ പുതിയ കോഡ് തനിയെ ബിൽഡ് ആയി ലോഡ് ബാലൻസറിൽ അപ്‌ഡേറ്റ് ആയിട്ടുണ്ടാകും!

**സീക്രട്ട്സ് എല്ലാം ആഡ് ചെയ്തോ ചേട്ടായി?** പൈപ്പ്‌ലൈൻ റൺ ആകുന്നുണ്ടോ എന്ന് നോക്കി പറയണേ! വല്ല എററും വരികയാണെങ്കിൽ ഉടൻ അറിയിക്കണേ!

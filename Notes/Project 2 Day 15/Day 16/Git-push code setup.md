
### **2. CLI വഴി കോഡ് പുഷ് ചെയ്യാനുള്ള സ്റ്റെപ്പുകൾ**

ആദ്യം ന്റെ ചേട്ടായി ഗിറ്റ്ഹബ്ബിൽ പോയി ഒരു **New Repository** ക്രിയേറ്റ് ചെയ്യുക (README ഫയലോ ലൈസൻസോ ഒന്നും ഇപ്പോൾ ആഡ് ചെയ്യേണ്ട, വെറും ബ്ലാങ്ക് റെപ്പോസിറ്ററി മതി). അതിനുശേഷം ടെർമിനലിൽ താഴെ പറയുന്ന കമാൻഡുകൾ ഓരോന്നായി അടിക്കുക:

**Step A: ഗിറ്റ് ഇനിഷ്യലൈസ് ചെയ്യുക**
```bash
cd ~/sentiments-api-project
git init
```

**Step B: അനാവശ്യ ഫയലുകൾ ഒഴിവാക്കാൻ `.gitignore` ഉണ്ടാക്കാം**
ഡോക്കർ ഇമേജുകളോ താൽക്കാലിക ഫയലുകളോ പുഷ് ചെയ്യാതിരിക്കാൻ ഇത് സഹായിക്കും.
```bash
cat <<EOF > .gitignore
__pycache__/
*.pyc
venv/
.env
.DS_Store
EOF
```

**Step C: ഫയലുകൾ ആഡ് ചെയ്യുക**
```bash
git add .
git commit -m "Initial commit: Sentiment Analysis API with Helm and K8s"
```

**Step D: ഗിറ്റ്ഹബ്ബിലേക്ക് കണക്ട് ചെയ്യുക**
(ഇവിടെ `<YOUR_REPO_URL>` എന്ന ഭാഗത്ത് ന്റെ ചേട്ടായിയുടെ പുതിയ ഗിറ്റ്ഹബ് ലിങ്ക് കൊടുക്കുക).
```bash
git branch -M main
git remote add origin <YOUR_REPO_URL>
git push -u origin main
```

---

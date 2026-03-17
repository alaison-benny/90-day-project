പുതിയ Ubuntu EC2-ൽ production-ready tools install ചെയ്യുന്നത് project-ന്റെ foundation ആണ്. Python + venv കൂടി ചേർക്കുന്നത് development, testing, debugging എല്ലാം smooth ആക്കാൻ സഹായിക്കും.  

---

### 🛠️ Base Setup – Step by Step  

#### 1. System Update
```bash
sudo apt update && sudo apt upgrade -y
```

#### 2. Git + GitHub Login
```bash
sudo apt install git -y
git config --global user.name "alaison-benny"
git config --global user.email "alisaalaison@gmail.com"
```
👉 GitHub CLI (gh) Installation and GitHub login 

Bash
sudo apt update && sudo apt install gh -y
ഇതിന് ശേഷം gh auth login എന്ന് ടൈപ്പ് ചെയ്ത് ലോഗിൻ ചെയ്യാം.

---

#### 3. Docker
```bash
sudo apt install ca-certificates curl gnupg -y
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
```
👉 Debugging:  
- `docker ps` → permission denied →  
```bash
sudo usermod -aG docker $USER
newgrp docker
```

---

#### 4. AWS CLI v2

unzip ഇൻസ്റ്റാൾ ചെയ്യാം
താഴെ പറയുന്ന കമാൻഡ് നൽകുക:

```bash
sudo apt update && sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install
```

#### 3. കൺഫേം ചെയ്യുക (The Debugging Step)

```bash
aws --version
```

#### അടുത്ത പ്രധാന സ്റ്റെപ്പ് (AWS Configuration):
AWS CLI ഇൻസ്റ്റാൾ ചെയ്തത് കൊണ്ട് മാത്രം കാര്യമായില്ല, ചേട്ടായിയുടെ AWS അക്കൗണ്ടിലേക്ക് കണക്ട് ചെയ്യാൻ Access Key, Secret Key എന്നിവ നൽകണം.

ചേട്ടായിയുടെ കയ്യിൽ IAM User-ന്റെ Access Key റെഡി ആണോ? ഉണ്ടെങ്കിൽ നമുക്ക് താഴെ പറയുന്ന കമാൻഡ് അടിച്ച് കോൺഫിഗർ ചെയ്യാം:

```bash
aws configure
```
---

#### 5. kubectl (Kubernetes CLI)
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

---

#### 6. Terraform
```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install terraform -y
terraform -version
```

---

#### 7. Python + venv
Ubuntu EC2-യിൽ Python3 default ആയി ഉണ്ടാകും, പക്ഷേ venv install ചെയ്യണം:
```bash
sudo apt install python3 python3-pip python3-venv -y
python3 --version
pip3 --version
```

👉 Virtual environment create ചെയ്യാൻ:
```bash
python3 -m venv myenv
source myenv/bin/activate
```
Deactivate ചെയ്യാൻ:
```bash
deactivate
```

---

### ✅ Next Step  
ഇപ്പോൾ EC2-ൽ GitHub, Docker, AWS CLI, kubectl, Terraform, Python, venv എല്ലാം install ചെയ്തു.  

👉 ഇനി project തുടങ്ങുമ്പോൾ:  
1. GitHub repo clone ചെയ്യാം.  
2. Terraform ഉപയോഗിച്ച് AWS EKS cluster build ചെയ്യാം.  

-----------------------------

ഇനി GitHub repo clone ചെയ്ത് cluster setup തുടങ്ങാം.  

### 🛠️ Production Context  
Real-world production-ൽ GitHub repo clone ചെയ്യുമ്പോൾ:  
- **Access Method**: HTTPS vs SSH. Production-ൽ SSH keys ഉപയോഗിക്കുന്നത് secure ആണ്.  
- **Branch Strategy**: `main` branch production-ready code, `dev` branch testing.  
- **CI/CD Integration**: Clone ചെയ്ത repo pipelines-ലേക്ക് connect ചെയ്യാം (GitHub Actions, Jenkins).  

---

### 📋 Step-by-Step Commands  

#### 1. Repo Clone (SSH recommended)
```bash
git clone git@github.com:alaison-benny/sentiments-api-project.git
```

👉 SSH key GitHub-ൽ add ചെയ്തിട്ടില്ലെങ്കിൽ HTTPS ഉപയോഗിക്കാം:
```bash
git clone https://github.com/alaison-benny/sentiments-api-project.git
```

#### 2. Repo Verify
```bash
cd sentiments-api-project
ls -la
git status
```

👉 Debugging:  
- **Error: Permission denied (publickey)** → SSH key GitHub-ൽ add ചെയ്തിട്ടില്ല.  
- **Error: Could not resolve host** → Internet/DNS issue.  
- **Error: Repository not found** → URL തെറ്റാണ് അല്ലെങ്കിൽ access ഇല്ല.  

---

### ✅ Next Step  
Repo clone ചെയ്ത ശേഷം, cluster setup തുടങ്ങാം. അതായത് **Terraform + AWS EKS**.  

👉 Production-ൽ Terraform workflow:  
1. `terraform init` → plugins download  
2. `terraform plan` → infra preview  
3. `terraform apply` → infra create  

repo clone command run ചെയ്ത് verify ചെയ്തോ? Repo-ൽ പ്രവേശിച്ചാൽ, ഞാൻ Terraform setup തുടങ്ങാൻ detailed production workflow കൊടുക്കാം. 🚀

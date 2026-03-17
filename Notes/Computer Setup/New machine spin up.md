പുതിയ Ubuntu EC2 instance-ൽ production-ready tools install ചെയ്യുന്നത് **foundation step** ആണ്. നാളെ project build ചെയ്യുമ്പോൾ debugging avoid ചെയ്യാൻ, ഇപ്പോൾ തന്നെ base setup ശരിയായി ചെയ്യണം.  

### 🛠️ Production Context  
Real-world production EC2-കളിൽ software install ചെയ്യുമ്പോൾ ചില കാര്യങ്ങൾ ശ്രദ്ധിക്കണം:  
- **Version locking**: AWS EC2-യിൽ default repos പലപ്പോഴും പഴയ versions നൽകും. അതിനാൽ Docker, Terraform, AWS CLI, kubectl എന്നിവയ്ക്ക് official install scripts ഉപയോഗിക്കണം.  
- **IAM roles vs Access Keys**: Production-ൽ EC2-യ്ക്ക് IAM role attach ചെയ്യുന്നത് best practice ആണ്. Access keys avoid ചെയ്യുക.  
- **Security**: GitHub login ചെയ്യുമ്പോൾ SSH keys ഉപയോഗിക്കുക, password-based login avoid ചെയ്യുക.  

---

### 📋 Step-by-Step Installation  

#### 1. Update System
```bash
sudo apt update && sudo apt upgrade -y
```

#### 2. Git + GitHub Login
```bash
sudo apt install git -y
git config --global user.name "ala"
git config --global user.email "your_email@example.com"
```
👉 Production-ൽ GitHub login SSH key ഉപയോഗിച്ച് ചെയ്യുക:
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
cat ~/.ssh/id_ed25519.pub
```
ഈ public key GitHub → Settings → SSH Keys-ൽ add ചെയ്യുക.

---

#### 3. Docker Install
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
- `docker ps` run ചെയ്യുമ്പോൾ permission denied error വന്നാൽ →  
```bash
sudo usermod -aG docker $USER
newgrp docker
```

---

#### 4. AWS CLI v2
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```
👉 Debugging: `aws configure` run ചെയ്ത് credentials set ചെയ്യുക. Production-ൽ IAM role attach ചെയ്യുന്നത് best practice.

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

### ✅ Next Step  
ഇപ്പോൾ EC2-ൽ GitHub, Docker, AWS CLI, kubectl, Terraform എല്ലാം install ചെയ്തു.  

👉 നാളെ രാവിലെ തുടങ്ങുമ്പോൾ:  
1. GitHub repo clone ചെയ്യാം.  
2. Terraform ഉപയോഗിച്ച് AWS EKS cluster build ചെയ്യാം.  


നാളെ തുടങ്ങുമ്പോൾ GitHub repo clone ചെയ്യുന്നതിൽ നിന്ന് തുടങ്ങട്ടെ?



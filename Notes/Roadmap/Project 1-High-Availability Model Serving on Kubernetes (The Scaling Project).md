### Project 2 (High-Availability Model Serving on Kubernetes) 

This project തിരഞ്ഞെടുക്കാനുള്ള നിങ്ങളുടെ തീരുമാനം വളരെ മികച്ചതാണ്. ബാംഗ്ലൂരിലെ കമ്പനികൾ ഏറ്റവും കൂടുതൽ ഡിമാൻഡ് ചെയ്യുന്ന ഒരു സ്കിൽ ആണിത്.

നിങ്ങൾ ഒരു **Scrum Master** ആയിരുന്നതുകൊണ്ട്, ഇതിനെ ഒരു 2-വീക്ക് സ്പ്രിന്റ് (Sprint) ആയി കാണുക. നിങ്ങളുടെ AWS Free Tier ഉപയോഗിച്ച് ഇത് എങ്ങനെ പ്രായോഗികമായി ചെയ്യാം എന്നതിൻ്റെ പ്ലാൻ താഴെ നൽകുന്നു:

---

### Project 2: High-Availability Model Serving on Kubernetes (Implementation Plan)

ഈ പ്രോജക്റ്റിനെ നമുക്ക് 5 ഘട്ടങ്ങളായി തിരിക്കാം:

#### ഘട്ടം 1: ആപ്ലിക്കേഷൻ നിർമ്മാണം (The AI API)

ആദ്യം ഒരു സിമ്പിൾ ML മോഡലിനെ API ആക്കി മാറ്റണം.

* **ചെയ്യേണ്ടത്:** Python-ൽ **FastAPI** ഉപയോഗിച്ച് ഒരു പ്രെഡിക്ഷൻ എൻഡ്‌പോയിന്റ് ഉണ്ടാക്കുക. ഒരു ഹൗസ് പ്രൈസ് പ്രെഡിക്ഷൻ മോഡലോ (Scikit-learn) അല്ലെങ്കിൽ വാല്യൂ നൽകിയാൽ തിരിച്ച് റിസൾട്ട് നൽകുന്ന ഒരു ഫങ്ക്ഷനോ മതിയാകും.
* **Gemini സഹായം:** *"Create a simple FastAPI application to serve a pre-trained Scikit-learn model."*

#### ഘട്ടം 2: കണ്ടെയ്‌നറൈസേഷൻ (Dockerization)

ഈ ആപ്ലിക്കേഷനെ എവിടെയും റൺ ചെയ്യാവുന്ന ഒരു പാക്കേജ് ആക്കി മാറ്റുക.

* **ചെയ്യേണ്ടത്:** ഒരു `Dockerfile` എഴുതുക. ആപ്ലിക്കേഷൻ ബിൽഡ് ചെയ്ത് **Docker Hub**-ലേക്കോ **AWS ECR** (Elastic Container Registry)-ലേക്കോ പുഷ് ചെയ്യുക.
* **Gemini സഹായം:** *"Write an optimized Dockerfile for a FastAPI ML application."*

#### ഘട്ടം 3: ഇൻഫ്രാസ്ട്രക്ചർ (EKS Setup using Terraform)

AWS-ൽ കുബെർനെറ്റീസ് ക്ലസ്റ്റർ സെറ്റപ്പ് ചെയ്യുക.

* **ചെയ്യേണ്ടത്:** ടെറാഫോം (Terraform) ഉപയോഗിച്ച് ഒരു **EKS Cluster** ക്രിയേറ്റ് ചെയ്യുക. 2 നോഡുകൾ (Nodes) എങ്കിലും ഉണ്ടെന്ന് ഉറപ്പാക്കുക.
* **ശ്രദ്ധിക്കുക:** EKS Free Tier-ൽ ഉൾപ്പെടില്ല. അതിനാൽ പഠിക്കാൻ വേണ്ടി നിങ്ങളുടെ ലോക്കൽ മെഷീനിൽ **Minikube** ഉപയോഗിക്കാം. ഇന്റർവ്യൂവിൽ "Terraform used for EKS" എന്ന് കാണിച്ചാൽ മതി.
* **Gemini സഹായം:** *"Terraform script to create a simple AWS EKS cluster with 2 worker nodes."*

#### ഘട്ടം 4: ഡിപ്ലോയ്മെന്റ് (Kubernetes Manifests & Helm)

യഥാർത്ഥ പ്ലാറ്റ്‌ഫോം എഞ്ചിനീയറിംഗ് ഇവിടെയാണ് നടക്കുന്നത്.

* **ചെയ്യേണ്ടത്:** `Deployment.yaml`, `Service.yaml` എന്നിവ തയ്യാറാക്കുക.
* **High Availability:** ഇതിൽ **Horizontal Pod Autoscaler (HPA)** സെറ്റപ്പ് ചെയ്യുക. അതായത് ട്രാഫിക് കൂടുമ്പോൾ 2 പോഡുകളിൽ (Pods) നിന്ന് 10 എണ്ണമായി തനിയെ കൂടണം.
* **Gemini സഹായം:** *"Create a Kubernetes Deployment and Service YAML with HPA (Horizontal Pod Autoscaler) for my API."*

#### ഘട്ടം 5: ലോഡ് ബാലൻസിംഗ് & ടെസ്റ്റിംഗ്

* **ചെയ്യേണ്ടത്:** AWS Load Balancer വഴി ആപ്പിനെ പുറംലോകത്തിന് ലഭ്യമാക്കുക. **Locust** അല്ലെങ്കിൽ **JMeter** ഉപയോഗിച്ച് ഒരു ലോഡ് ടെസ്റ്റ് നടത്തി സ്കെയിലിംഗ് വർക്ക് ചെയ്യുന്നുണ്ടോ എന്ന് ഉറപ്പുവരുത്തുക.

---

### ദിവസേനയുള്ള ആക്ഷൻ പ്ലാൻ (12 Hours Schedule)

| ദിവസം | ലക്ഷ്യം (Goal) |
| --- | --- |
| **ദിവസം 1-2** | Python API & Docker പഠിക്കുക. നിങ്ങളുടെ ആപ്പിന്റെ ഡോക്കർ ഇമേജ് റെഡിയാക്കുക. |
| **ദിവസം 3-5** | Kubernetes Basics പഠിക്കുക. ലോക്കൽ മെഷീനിൽ Minikube ഇൻസ്റ്റാൾ ചെയ്ത് ആപ്പ് റൺ ചെയ്യുക. |
| **ദിവസം 6-8** | Terraform ഉപയോഗിച്ച് AWS-ൽ റിസോഴ്സുകൾ ഉണ്ടാക്കാൻ പഠിക്കുക. |
| **ദിവസം 9-11** | HPA, Load Balancer, Scaling എന്നിവ സെറ്റപ്പ് ചെയ്യുക. |
| **ദിവസം 12-14** | മൊത്തം സിസ്റ്റം ടെസ്റ്റ് ചെയ്യുക. GitHub-ൽ ഡോക്യുമെന്റേഷൻ തയ്യാറാക്കുക. |

---

നിങ്ങളുടെ കരിയർ മാറ്റവും, ഇപ്പോഴത്തെ സാഹചര്യവും, നമ്മൾ പ്ലാൻ ചെയ്ത പ്രോജക്റ്റും എല്ലാം ഉൾപ്പെടുത്തി Copilot-ന് നൽകാൻ പറ്റിയ വളരെ വ്യക്തമായ ഒരു **Prompt** താഴെ നൽകുന്നു. ഇത് കോപ്പി ചെയ്ത് നിങ്ങൾക്ക് ഉപയോഗിക്കാം.

---

### Copilot-നുള്ള Prompt

"Hi Copilot, I am a 35-year-old professional from Bangalore, transitioning my career from a **Scrum Master (6 years exp)** to an **AI/ML Platform Engineer (MLOps)**. I have basic knowledge of Linux, Docker, Kubernetes, Terraform, and AWS. Due to a recent layoff, I am currently dedicated to mastering these skills by investing 12 hours daily to get a job as soon as possible.

I am working on a production-grade project: **'Eat Love Explore AI'** — a RAG-based Food Prediction Chatbot.

**Project Overview:**
The app uses FastAPI, LangChain, FAISS (Vector Store), and Ollama (Llama3 model) to suggest food items based on a custom dataset.

**Current Goal:**
I want to deploy this as a high-availability service on Kubernetes.

**The Roadmap of my project is:**

1. **Containerization:** Wrap the FastAPI app and its dependencies using Docker.
2. **Local Orchestration:** Run the app and Ollama together using Docker Compose.
3. **Infrastructure as Code:** Provision an AWS EKS cluster (or local K8s like Minikube/Kind) using Terraform.
4. **Kubernetes Deployment:** Create Manifests (Deployments, Services, HPA) for scaling the app.
5. **CI/CD:** Automate the build and deploy process using GitHub Actions.

**How you can help me:**
I have the `app.py` code ready. Now, I need your step-by-step guidance to complete the **Dockerization and Kubernetes deployment**. Please provide explanations, commands, and YAML files.

**Important:** Please explain the technical concepts and provide the replies in **Malayalam**, as it helps me understand the logic better and learn faster.

Let's start with the first step: **How do I structure my project folder and write the requirements.txt and Dockerfile for this specific app?**"

---

### ഈ പ്രോംപ്റ്റ് നൽകിയാലുള്ള ഗുണങ്ങൾ:

* **Context:** നിങ്ങൾ ആരാണെന്നും നിങ്ങളുടെ ലക്ഷ്യം എന്താണെന്നും Copilot-ന് മനസ്സിലാകും.
* **Clarity:** പ്രോജക്റ്റിന്റെ ഘട്ടങ്ങൾ വ്യക്തമായതുകൊണ്ട് Copilot കൃത്യമായ പാതയിലൂടെ നിങ്ങളെ നയിക്കും.
* **Language:** മലയാളത്തിൽ മറുപടി വേണമെന്ന് പ്രത്യേകം പറഞ്ഞിട്ടുള്ളതിനാൽ പഠനം എളുപ്പമാകും.

**ഈ പ്രോംപ്റ്റ് ഉപയോഗിച്ച് Copilot-നോട് ചോദിച്ചു നോക്കൂ. അടുത്തതായി നമുക്ക് Docker ഇമേജ് ബിൽഡ് ചെയ്യുന്നതിനിടയിൽ വരുന്ന സംശയങ്ങൾ തീർക്കണോ, അതോ നേരിട്ട് K8s Manifests-ലേക്ക് കടക്കണോ?**

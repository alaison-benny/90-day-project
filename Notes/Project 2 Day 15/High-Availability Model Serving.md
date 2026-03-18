### **High-Availability Model Serving on Kubernetes (EKS) workflow**


1. **GitHub Repo Pull** – കോഡ് GitHub-ൽ നിന്ന് clone ചെയ്ത് local-ലോ EC2-ലോ കൊണ്ടുവരും. Production-ൽ repo access, branch strategy, CI/CD integration എന്നിവ explain ചെയ്യും.  
2. **Cluster Setup (Terraform + AWS EKS)** – IaC (Infrastructure as Code) ഉപയോഗിച്ച് scalable Kubernetes cluster build ചെയ്യും. Production-ൽ IAM roles, VPC networking, node groups, cost optimization എന്നിവ detail ചെയ്യും.  
3. **Deployment (Kubernetes manifests, ConfigMaps, Secrets)** – API deploy ചെയ്യുമ്പോൾ YAML files, environment configs, secrets management (AWS Secrets Manager vs Kubernetes Secrets) എന്നിവ explain ചെയ്യും.  
4. **Scaling (HPA)** – Horizontal Pod Autoscaler configure ചെയ്ത് load-നുസരിച്ച് pods auto-scale ചെയ്യുന്നത് കാണിക്കും. Debugging: metrics-server issues, resource limits misconfiguration.  
5. **Load Balancing (AWS ALB/Ingress Controller)** – Ingress + ALB setup ചെയ്ത് external traffic handle ചെയ്യുന്നത്. Real-world: SSL termination, path-based routing, DNS integration.  
6. **Monitoring (Prometheus Operator + Grafana)** – Cluster + API observability dashboards build ചെയ്യും. Debugging: scrape configs, missing metrics, Grafana datasource issues.  
7. **Helm Charts** – Production-ൽ repeatable deployments Helm charts ഉപയോഗിച്ച് ചെയ്യുന്നത്. Chart structure, values.yaml overrides, upgrades, rollback strategies.  
 

cd sentiments-api-project/k8s

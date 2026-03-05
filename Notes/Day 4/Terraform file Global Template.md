## ഒരു **Prompt Template** ഉപയോഗിക്കുന്നത് നിങ്ങളുടെ ജോലി വേഗത്തിലാക്കും. താഴെ നൽകിയിരിക്കുന്ന ടെംപ്ലേറ്റ് നിങ്ങൾക്ക് ഏത് AI മോഡലിലും (Gemini, ChatGPT, etc.) ഉപയോഗിക്കാം.

ഇത് നൽകുന്നതിലൂടെ കൃത്യമായ **HCL (Hashicorp Configuration Language)** നിയമങ്ങൾ പാലിച്ചുകൊണ്ടുള്ള ഫയലുകൾ നിങ്ങൾക്ക് ലഭിക്കും.

---

### **Global Terraform Generator Prompt Template**

ഈ താഴെ കാണുന്ന ഭാഗം കോപ്പി ചെയ്ത് സൂക്ഷിക്കുക. നിങ്ങൾക്ക് പുതിയൊരു ഇൻഫ്രാസ്ട്രക്ചർ വേണമെന്നുണ്ടെങ്കിൽ ഇതിലെ `Inputs` മാത്രം മാറ്റിയാൽ മതി.

> **PROMPT START**
> "I am a Senior Platform Engineer working on a **[Project Name]**. Create a production-ready Terraform configuration for AWS with the following details:
> **Inputs:**
> 1. **Region:** [e.g., us-east-1]
> 2. **Resources to Create:** [e.g., EC2 Instance, Security Group, S3 Bucket]
> 3. **Variable Names:** [e.g., instance_name, app_port, db_password]
> 4. **Ingress/Egress Ports:** [e.g., 22, 80, 8000]
> 
> 
> **Requirements:**
> * Provide separate code for `main.tf` and `variables.tf`.
> * Follow proper HCL indentation (2 spaces) and alignment for '=' signs.
> * Use the **Dry Principle** (Don't Repeat Yourself) by using variables instead of hardcoding values.
> * Add professional comments in English explaining each resource block.
> * Ensure the security group allows SSH and the specified app ports.
> 
> 
> Output the result in two distinct code blocks for easy copying."
> **PROMPT END**

---

### **ഇൻഡസ്ട്രിയിൽ ഈ ടെംപ്ലേറ്റ് എങ്ങനെ ഉപയോഗിക്കാം? (The Real-World Scenario)**

നമ്മുടെ **AI Food Delivery App**-നായി ഇത് ഒന്ന് പരീക്ഷിച്ചു നോക്കാം. `Inputs` ഭാഗത്ത് താഴെ പറയുന്നവ നൽകുക:

* **Project Name:** AI Food Delivery Infrastructure
* **Region:** us-east-1
* **Resources:** EC2 Instance (t3.medium), Security Group, S3 Bucket for AI Models.
* **Variable Names:** `instance_name`, `app_port`, `bucket_name`.
* **Ports:** 22, 8000 (API), 9090 (Prometheus).

---

### **എന്തുകൊണ്ട് ഈ പ്രോംപ്റ്റ് ടെംപ്ലേറ്റ് മികച്ചതാണ്?**

1. **Standardization:** ഓരോ തവണയും ഒരേ നിലവാരത്തിലുള്ള കോഡ് (Standardized Code) ഇത് നിങ്ങൾക്ക് നൽകുന്നു.
2. **Error Prevention:** വാല്യൂസ് കോഡിനുള്ളിൽ നേരിട്ട് എഴുതാതെ (Hardcoding) വേരിയബിളുകളാക്കി മാറ്റാൻ ഇത് AI-യെ നിർബന്ധിക്കുന്നു.
3. **Speed:** കൈകൊണ്ട് എഴുതുന്നതിനേക്കാൾ 10 മടങ്ങ് വേഗത്തിൽ ഇൻഫ്രാസ്ട്രക്ചർ കോഡ് തയ്യാറാക്കാം.

**അടുത്ത ഘട്ടം:**

നമ്മുടെ ടെറാഫോം സെക്ഷൻ ഏകദേശം പൂർത്തിയായി. ഇനി നമ്മൾ അടുത്ത വലിയ ഘട്ടമായ **Phase 1: Step 2 - Docker-based Kafka Setup**-ലേക്ക് കടക്കാൻ പോകുകയാണ്. ഓർഡറുകൾ തത്സമയം ട്രാക്ക് ചെയ്യാനും മൈക്രോ സർവീസുകൾ തമ്മിൽ സംസാരിക്കാനും ഇത് നമ്മളെ സഹായിക്കും.

**നിങ്ങൾ `terraform plan` കമാൻഡ് ചെക്ക് ചെയ്തു കഴിഞ്ഞോ? നമുക്ക് ഇനി Kafka-യിലേക്ക് പോകാമോ?**

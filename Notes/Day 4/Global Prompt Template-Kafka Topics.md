നിങ്ങൾ ഇപ്പോൾ **Terraform** ഉപയോഗിച്ച് സെക്യൂരിറ്റി ഗ്രൂപ്പുകൾ വിജയകരമായി നിർമ്മിച്ചു കഴിഞ്ഞു. ഇനി ഇതേ രീതിയിൽ **Kafka Topics** കൂടി ഓട്ടോമേറ്റഡ് ആയി നിർമ്മിക്കാൻ സഹായിക്കുന്ന ഒരു **Global Prompt Template** താഴെ നൽകുന്നു.

ഇത് ഉപയോഗിക്കുന്നതിലൂടെ ഓരോ തവണയും മാനുവലായി ടോപ്പിക്കുകൾ ക്രിയേറ്റ് ചെയ്യുന്നതിന് പകരം, പ്രൊഡക്ഷൻ നിലവാരത്തിലുള്ള കോഡ് നിങ്ങൾക്ക് ലഭിക്കും.

---

### **Kafka Topic Terraform Generator Template**

ഈ ഭാഗം കോപ്പി ചെയ്ത് സൂക്ഷിക്കുക. പുതിയൊരു ടോപ്പിക് വേണമെന്നുണ്ടെങ്കിൽ `Inputs` മാത്രം മാറ്റിയാൽ മതി.

> **PROMPT START**
> "I am a Senior Platform Engineer managing a Kafka cluster on AWS. Create a professional Terraform configuration to manage **Kafka Topics** with the following details:
> **Inputs:**
> 1. **Provider:** [e.g., Mongey/kafka or Confluent Cloud]
> 2. **Topic Names:** [e.g., order-events, payment-updates, delivery-tracking]
> 3. **Partitions:** [e.g., 3 for production, 1 for dev]
> 4. **Replication Factor:** [e.g., 2 or 3]
> 5. **Retention Period:** [e.g., 7 days in milliseconds]
> 
> 
> **Requirements:**
> * Provide separate code for `main.tf` and `variables.tf`.
> * Use **Variables** for all values (Partitions, Replication, Names) to follow the DRY principle.
> * Ensure proper HCL indentation (2 spaces) as we practiced.
> * Add comments explaining why these partition/replication counts are chosen for production.
> * Include an `outputs.tf` block to display the created topic names.
> 
> 
> Output the result in clearly separated code blocks."
> **PROMPT END**

---

### **ഇൻഡസ്ട്രിയിൽ ഇത് എങ്ങനെ പ്രവർത്തിക്കുന്നു? (The Flow)**

ഈ ടെംപ്ലേറ്റ് ഉപയോഗിക്കുമ്പോൾ നിങ്ങൾ ശ്രദ്ധിക്കേണ്ട ചില കാര്യങ്ങൾ:

* **Partitions:** ഇതിനെ ഒരു റോഡിലെ 'ലെയ്‌നുകൾ' (Lanes) ആയി കരുതാം. കൂടുതൽ ട്രാഫിക് ഉള്ള ടോപ്പിക്കുകൾക്ക് (ഉദാഹരണത്തിന് `delivery-tracking`) കൂടുതൽ പാർട്ടിഷനുകൾ നൽകുന്നത് സ്പീഡ് കൂട്ടാൻ സഹായിക്കും.
* **Replication Factor:** ഇത് ഒരു 'ബാക്കപ്പ്' പോലെയാണ്. ഒരു സെർവർ പണിമുടക്കിയാലും ഡാറ്റ നഷ്ടപ്പെടാതിരിക്കാൻ പ്രൊഡക്ഷനിൽ സാധാരണയായി ഇത് `3` ആണ് നൽകാറുള്ളത്.
* **Provider:** കാഫ്ക കൈകാര്യം ചെയ്യാൻ ടെറാഫോമിന് ഒരു 'ഹാൻഡ്‌ലർ' (Provider) ആവശ്യമാണ്. സാധാരണയായി `Mongey/kafka` എന്ന പ്രൊവൈഡർ ആണ് ഇതിനായി ഉപയോഗിക്കുന്നത്.

---

### **റിയൽ വേൾഡ് എക്സാമ്പിൾ: Swiggy/Zomato Scenario**

നിങ്ങൾ ഈ പ്രോംപ്റ്റിൽ `Inputs` ആയി താഴെ പറയുന്നവ നൽകിയാൽ കിട്ടുന്ന ഗുണം:

* **Topic Name:** `food-delivery-status`
* **Partitions:** `3`
* **Result:** ഒരേസമയം മൂന്ന് ഡെലിവറി ബോയ്സ് സ്റ്റാറ്റസ് അപ്‌ഡേറ്റ് ചെയ്താലും സിസ്റ്റം ഹാങ്ങ് ആകാതെ വളരെ വേഗത്തിൽ അത് പ്രോസസ്സ് ചെയ്യപ്പെടും.

**അടുത്ത നടപടി:**

നിങ്ങളുടെ കൈവശം ഇപ്പോൾ Kafka-യും Zookeeper-ഉം റൺ ചെയ്യുന്ന ഒരു സിസ്റ്റം ഉണ്ട്.

**നമുക്ക് ഇനി നമ്മൾ നേരത്തെ നിർമ്മിച്ച `food-orders` എന്ന ടോപ്പിക്കിലേക്ക് ഒരു മെസ്സേജ് അയച്ച് അത് മറ്റൊരു വിൻഡോയിൽ സ്വീകരിക്കുന്നത് (Consume) എങ്ങനെയാണെന്ന് നോക്കിയാലോ?** ഇത് ചെയ്യുന്നതോടെ ഒരു ഡെലിവറി ആപ്പ് എങ്ങനെയാണ് ബാക്കെൻഡിൽ പ്രവർത്തിക്കുന്നത് എന്ന് നിങ്ങൾക്ക് നേരിട്ട് ബോധ്യപ്പെടും. തുടരാമോ?

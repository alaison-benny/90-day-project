### 4) Hardcoded path to dynamic changing practice example

നല്ല ചോദ്യം ചേട്ടായി! ഒരു പ്ലാറ്റ്‌ഫോം എൻജിനീയർക്ക് തന്റെ ജോലി എളുപ്പമാക്കാൻ AI (Gemini അല്ലെങ്കിൽ ChatGPT) എങ്ങനെ ഉപയോഗിക്കാം എന്ന് നമുക്ക് നോക്കാം. പലപ്പോഴും വലിയ കോഡുകളിൽ എവിടെയൊക്കെയാണ് പാത്തുകൾ ഒളിച്ചിരിക്കുന്നത് എന്ന് കണ്ടുപിടിക്കാൻ AI നമ്മളെ സഹായിക്കും.

ചേട്ടായിക്ക് AI-യോട് താഴെ പറയുന്ന രീതിയിലുള്ള **Prompts** ചോദിക്കാവുന്നതാണ്:

---

### 1. Hardcoded Paths കണ്ടുപിടിക്കാൻ (Finding the paths)
ചേട്ടായിയുടെ കയ്യിൽ ഒരു വലിയ പൈത്തൺ ഫയൽ ഉണ്ടെങ്കിൽ AI-യോട് ഇങ്ങനെ ചോദിക്കാം:
> *"I have this Python code. Can you find all the hardcoded paths in it and list them?"*

**AI എന്ത് ചെയ്യും:** കോഡ് മുഴുവൻ വായിച്ച് `"/home/ubuntu..."`, `"C:/Users..."` തുടങ്ങിയ ഭാഗങ്ങൾ സെലക്ട് ചെയ്ത് ചേട്ടായിക്ക് തരും.

---

### 2. കോഡിനെ പ്രൊഡക്ഷൻ റെഡി ആക്കാൻ (Making it Production-ready)
ഒരു ഡാറ്റാ സയന്റിസ്റ്റ് തന്ന കോഡ് AI-ക്ക് നൽകിയിട്ട് ഇങ്ങനെ പറയാം:
> *"Make this Python code production-ready by replacing hardcoded paths with environment variables using the `os` library. Use meaningful names like `MODEL_PATH` and `DB_URL`."*

**AI എന്ത് ചെയ്യും:** ചേട്ടായിക്ക് വേണ്ടി `import os` ചേർക്കുകയും, നമ്മൾ നേരത്തെ പഠിച്ച `os.getenv()` വരികൾ എഴുതിത്തരികയും ചെയ്യും.



---

### 3. ഡോക്കറിന് വേണ്ടി മാറ്റാൻ (For Containerization)
ചേട്ടായി ഒരു ആപ്പിനെ ഡോക്കറിലേക്ക് മാറ്റുകയാണെങ്കിൽ ഇങ്ങനെ ചോദിക്കാം:
> *"Rewrite these paths to work inside a Docker container. Use a base directory and join paths dynamically."*

**AI എന്ത് ചെയ്യും:** `BASE_DIR = os.path.dirname(os.path.abspath(__file__))` എന്ന വരി ഉപയോഗിച്ച് കോഡ് മാറ്റിത്തരും.

---

### 4. ഉദാഹരണത്തിന് (Example)
ചേട്ടായി AI-ക്ക് ഈ വരി നൽകുന്നു എന്ന് വിചാരിക്കുക:
`DATA = "/Users/chittayi/Desktop/project/data.csv"`

AI ഇതിനെ ഇങ്ങനെ മാറ്റിത്തരും:
```python
import os
# AI നൽകുന്ന മറുപടി:
DATA = os.getenv("DATA_FILE_PATH", "./data.csv")
```

---


### എങ്ങനെ സുരക്ഷിതമായി AI ഉപയോഗിക്കാം? (How to overcome this?)

ഒരു പ്ലാറ്റ്‌ഫോം എൻജിനീയർ എന്ന നിലയിൽ ചേട്ടായി താഴെ പറയുന്ന 4 വഴികൾ ഉപയോഗിക്കണം:

#### 1. കോഡിനെ "അനോണിമൈസ്" ചെയ്യുക (Anonymize the code)
യഥാർത്ഥ അഡ്രസ്സുകളും പേരുകളും മാറ്റിയിട്ട് മാത്രം AI-യോട് ചോദിക്കുക.
* **യഥാർത്ഥ കോഡ്:** `/home/praveen/projects/secret_ai/v1/model.pkl`
* **AI-ക്ക് നൽകുന്നത്:** `/path/to/my/folder/model.pkl`
ഇങ്ങനെ മാറ്റിയാൽ ചേട്ടായിയുടെ ഫോൾഡർ പേരോ യൂസർ നെയിമോ AI അറിയില്ല.

#### 2. രഹസ്യങ്ങൾ (Secrets) ഒരിക്കലും നൽകരുത്
കോഡിൽ API കീകൾ, പാസ്‌വേഡുകൾ, ഡാറ്റാബേസ് ലിങ്കുകൾ എന്നിവ ഉണ്ടെങ്കിൽ അവ മാറ്റി `YOUR_SECRET_KEY` എന്ന് ടൈപ്പ് ചെയ്ത ശേഷം മാത്രം AI-ക്ക് കോഡ് നൽകുക.

#### 3. ലോക്കൽ AI ഉപയോഗിക്കുക (Local/Private LLMs)
വലിയ കമ്പനികൾ ചാറ്റ് ജിപിറ്റിക്ക് പകരം അവരുടെ സ്വന്തം സർവറിൽ മാത്രം വർക്ക് ചെയ്യുന്ന AI (ഉദാഹരണത്തിന് **Llama** അല്ലെങ്കിൽ **Ollama**) ഉപയോഗിക്കും. ഇത് ഇന്റർനെറ്റുമായി ബന്ധമില്ലാത്തതുകൊണ്ട് വിവരങ്ങൾ പുറത്തുപോകില്ല.

#### 4. കോഡ് കഷണങ്ങൾ (Snippets) മാത്രം നൽകുക
മുഴുവൻ ഫയലും നൽകാതെ, ചേട്ടായിക്ക് സംശയമുള്ള 2-3 വരികൾ മാത്രം നൽകി ചോദിക്കുക.
* **ഉദാഹരണം:** *"How to convert this specific line `/data/file.csv` to an environment variable?"* എന്ന് മാത്രം ചോദിക്കുക.



---


### 📝 പരിശീലന ചോദ്യങ്ങൾ (Practice Problems)

**ചോദ്യം 1: മോഡൽ പാത്ത് മാറ്റുക (Model Path)**
ഒരു ഡാറ്റാ സയന്റിസ്റ്റ് തന്റെ കോഡിൽ ഇങ്ങനെ എഴുതിയിരിക്കുന്നു:
`my_model_loc = "/home/ubuntu/ai_project/v1/model.pkl"`
ഇതിനെ **`MODEL_PATH`** എന്ന ലേബൽ (Environment Variable) ഉപയോഗിച്ച് പ്ലാറ്റ്‌ഫോം എൻജിനീയറിംഗ് രീതിയിലേക്ക് മാറ്റുക. (സർവറിൽ ഈ ലേബൽ ഇല്ലെങ്കിൽ `./models/default.pkl` എന്ന് എടുക്കണം).

**ചോദ്യം 2: ഡാറ്റാബേസ് പാത്ത് മാറ്റുക (Database Path)**
കോഡിൽ ഇങ്ങനെയാണ് ഉള്ളത്:
`DB_FILE = "C:/Users/Praveen/Documents/database.sqlite"`
ഇതിനെ **`DATABASE_URL`** എന്ന പേര് ഉപയോഗിച്ച് മാറ്റുക. ഇതിന്റെ കൂടെ `import os` ചേർക്കാനും മറക്കരുത്.

**ചോദ്യം 3: ലോഗ് ഫയൽ മാറ്റുക (Log File)**
`LOG_LOCATION = "/var/www/app/logs/error.log"`
ഇതിനെ **`APP_LOG_PATH`** എന്ന പേരിൽ മാറ്റുക. സർവറിൽ സെറ്റിംഗ്സ് ഇല്ലെങ്കിൽ വെറും `error.log` എന്ന് എടുക്കണം.

**ചോദ്യം 4: നിലവിലെ ഫോൾഡർ കണ്ടുപിടിക്കുക (Dynamic BASE_DIR)**
കോഡ് എവിടെയാണോ റൺ ചെയ്യുന്നത്, ആ ഫോൾഡറിനുള്ളിലെ `config.json` എന്ന ഫയലിന്റെ പാത്ത് കണ്ടുപിടിക്കാൻ `os.path` ഉപയോഗിച്ച് ഒരു വരി എഴുതുക. (Hint: `BASE_DIR` ആദ്യം കണ്ടുപിടിക്കണം).

---

Question1 answer:
MODEL_PATH = os.getenv("MODEL-PATH", "./default.pkl")

Question2 answer:
DATABASE_URL = os.getenv("DATABASE_URL", "./database.sqlite")

question 3 answer
APP_LOG_PATH = os.getenv("APP_LOG_PATH", "./error.log")

question 4 answer
BASE_DIR = os.path.dirname(os.path.abspath(__file__))

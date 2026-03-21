I want to learn AI/ML platform engineering role from scratch. So, teach me blow given coding concepts with real life examples in simple language. Teach me below given each concepts with the blow given sample python code. Teach me each concept step by step how to write with deep theory, then show me 4 examples, detail explain each part in the code in the example. Do this to each concepts below like an infinite loop until I say I learned this concept and let's move to next concept. Don't move to the next concept until I say that I learned this and let's move to next. Teach me in simple language with real life examples like a Teacher teaching her student. Teach me in Malayalam. Then finally after I say that I learned the concept, then give me 4 problems to do my self then give me the answers for that 2 problems in the next chat.

Platform engineer coding concepts are"
1. എന്ത് മാറ്റണം? (What to change?)
ഒരു ഡാറ്റാ സയന്റിസ്റ്റ് നൽകുന്ന കോഡിൽ താഴെ പറയുന്നവ മാറ്റണം:
Hardcoded Paths: /home/ubuntu/... പോലുള്ള പാത്തുകൾ മാറ്റി os.environ അല്ലെങ്കിൽ config ഫയലുകൾ ഉപയോഗിക്കണം.
Print Statements: പ്രൊഡക്ഷനിൽ print ആരും കാണില്ല. പകരം logging ലൈബ്രറി ഉപയോഗിച്ച് എററുകൾ രേഖപ്പെടുത്തണം.
Secrets & Keys: പാസ്‌വേഡുകൾ, API കീകൾ എന്നിവ കോഡിൽ നിന്ന് മാറ്റി Secret Managers (AWS Secrets Manager) വഴിയാക്കണം.
Error Handling: കോഡ് ക്രാഷ് ആകാതിരിക്കാൻ try-except ബ്ലോക്കുകൾ കൂടുതൽ കൃത്യമാക്കണം.
2. എങ്ങനെ മാറ്റണം? (How to change?)
ഇതൊരു സിസ്റ്റമാറ്റിക് പ്രോസസ്സാണ്:
Environment Variables: MODEL_PATH = os.getenv("MODEL_PATH", "/default/path") എന്ന രീതിയിൽ മാറ്റുക.
Validation: വരുന്നത് വെറും ലിസ്റ്റ് ആണോ അതോ ശരിയായ ഫ്ലോട്ട് വാല്യൂസ് ആണോ എന്ന് Pydantic പോലുള്ള ലൈബ്രറി വെച്ച് ചെക്ക് ചെയ്യുക.
Port & Host Configuration: സർവർ ഏത് പോർട്ടിൽ റൺ ചെയ്യണമെന്ന് കോഡിൽ ഉറപ്പിക്കാതെ അത് പുറമെ നിന്ന് (Docker വഴി) നൽകുക.
3. എപ്പോൾ മാറ്റണം? (When to change?)
During Containerization: കോഡ് ഒരു ഡോക്കർ ഇമേജ് ആക്കി മാറ്റുന്ന സമയത്താണ് ഏറ്റവും കൂടുതൽ മാറ്റങ്ങൾ വരുത്തേണ്ടി വരിക.
Cloud Deployment: സിസ്റ്റം AWS-ലേക്കോ അസൂറിലേക്കോ മാറ്റുമ്പോൾ ക്ലൗഡ് സർവീസുകളുമായി കണക്ട് ചെയ്യാൻ മാറ്റങ്ങൾ വേണ്ടിവരും.
Scaling: ആപ്ലിക്കേഷനിലേക്ക് ഒരേസമയം ആയിരക്കണക്കിന് ആളുകൾ വരുമ്പോൾ പെർഫോമൻസ് കൂട്ടാനായി മാറ്റങ്ങൾ വരുത്തണം.
4. AI (Gemini/ChatGPT) എങ്ങനെ ഉപയോഗിക്കാം?
പലപ്പോഴും നമ്മൾ മറന്നുപോകുന്ന കാര്യങ്ങൾ ഓർമ്മിപ്പിക്കാൻ AI-ക്ക് കഴിയും. ചേട്ടായിക്ക് താഴെ പറയുന്ന രീതിയിൽ AI-യോട് ചോദിക്കാം:
Scenario 1 (Security): "Make this Python code production-ready by adding logging and environment variables."
Scenario 2 (Optimization): "Convert this hardcoded MLflow path to a dynamic path that works in a Docker container."
Scenario 3 (Debugging): "I'm getting a connection error in FastAPI while running in EC2. Help me debug the network settings."
Scenario 4 (Health Checks): "Write a Kubernetes health check endpoint for this FastAPI code."
5. റിയൽ ലൈഫ് കമ്പനികളിൽ മാറ്റേണ്ട പ്രധാന കാര്യങ്ങൾ:
Scalability: ഒരേസമയം ഒന്നിലധികം മോഡലുകൾ ലോഡ് ചെയ്യണമെങ്കിൽ അതിനുള്ള സൗകര്യം കോഡിൽ വരുത്തണം.
Security: ആരെങ്കിലും വന്ന് സിസ്റ്റം ഹാക്ക് ചെയ്യാതിരിക്കാൻ Authentication (JWT) ചേർക്കണം.
Telemetry: ആപ്പിന് എത്ര മെമ്മറി വേണം, എത്ര സമയം കൊണ്ട് ഒരു റിസൾട്ട് തരുന്നു എന്നതൊക്കെ അറിയാൻ Monitoring Tools ലിങ്ക് ചെയ്യണം.
Versioning: പഴയ മോഡൽ മാറ്റി പുതിയത് വരുമ്പോൾ ആപ്പ് ഡൗൺ ആകാതെ അപ്ഡേറ്റ് ചെയ്യാൻ കോഡ് മാറ്റണം."


Sample Python code is "import mlflow.sklearn
import os
import logging # [PE EDIT: Standard logging ഉപയോഗിക്കണം, വെറും print പോരാ]
from fastapi import FastAPI
import uvicorn

# [PE EDIT: Logging configuration ഇവിടെ സെറ്റ് ചെയ്യും]
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

app = FastAPI()

# [PE EDIT: Hardcoded path മാറ്റണം! ഇത് Environment Variable ആക്കും]
# /home/ubuntu/... എന്നത് മറ്റൊരു സർവറിൽ വർക്ക് ആകില്ല.
# ഇതിന് പകരം: MODEL_PATH = os.getenv("MODEL_PATH", "default/path")
MODEL_PATH = "/home/ubuntu/mlruns/1/models/m-553ae89e3eb44e41a87c2cbcca7bc524/artifacts"

# [PE EDIT: Startup Logic - മോഡൽ ലോഡ് ചെയ്യുന്നതിൽ പിശക് വന്നാൽ 
# ആപ്പ് സ്റ്റാർട്ട് ആകാതെ തന്നെ 'Crash Loop' ആയി നിൽക്കാൻ പാകത്തിന് മാറ്റും]
try:
    model = mlflow.sklearn.load_model(MODEL_PATH)
    logger.info("✅ മോഡൽ വിജയകരമായി ലോഡ് ചെയ്തു!")
except Exception as e:
    logger.error(f"❌ ലോഡിംഗ് പരാജയം: {e}")
    # [PE EDIT: ഇവിടെ ഒരു 'raise' കൊടുത്ത് സർവർ റൺ ആകുന്നത് തടയാം, അല്ലെങ്കിൽ ശൂന്യമായ മോഡൽ വരാൻ പാടില്ല]

# [PE EDIT: Health Check Endpoint - Kubernetes-ന് ആപ്പ് ജീവനോടെ ഉണ്ടോ എന്ന് നോക്കാൻ]
@app.get("/health")
def health_check():
    return {"status": "healthy", "model_loaded": model is not None}

@app.get("/")
def home():
    return {"message": "API is running"}

@app.post("/predict")
def predict(data: dict):
    # [PE EDIT: Input validation - തെറ്റായ ഡാറ്റ വന്നാൽ സർവർ ക്രാഷ് ആകാതിരിക്കാൻ check ചെയ്യും]
    try:
        features = data.get("inputs")
        if features is None:
            return {"error": "No input data provided"}, 400
            
        prediction = model.predict(features)
        return {"prediction": prediction.tolist()}
    except Exception as e:
        logger.error(f"Prediction error: {e}")
        return {"error": "Internal server error during prediction"}, 500

# [PE EDIT: Uvicorn settings - പ്രൊഡക്ഷനിൽ worker threads, timeout ഒക്കെ മാറ്റും]
if __name__ == "__main__":
    # Host and Port should be configurable via Environment Variables
    port = int(os.getenv("PORT", 8000))
    # '0.0.0.0' ഉപയോഗിക്കുന്നത് കണ്ടെയ്‌നറിൽ നിന്ന് പുറത്തേക്ക് ആക്‌സസ് കിട്ടാനാണ്.
    uvicorn.run(app, host="0.0.0.0", port=port)"

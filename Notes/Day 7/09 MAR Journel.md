### Today complete chaitha task 

Phase 2: AI & RAG Pipeline (ദിവസം 5 - 9)
AI മോഡലുകളുടെ നിർമ്മാണവും അവയുടെ വേഗതയും ഉറപ്പുവരുത്തുന്ന ഘട്ടം.

Vector DB (FAISS/ChromaDB): 1.5 ദിവസം. (മെനു വിവരങ്ങൾ എംബെഡിംഗുകളാക്കി മാറ്റാൻ).

LangChain & RAG: 2 ദിവസങ്ങൾ. (ക്യുറി പ്രോസസ്സിംഗ്, പ്രോംപ്റ്റ് എഞ്ചിനീയറിംഗ്).

MLflow Integration: 1.5 ദിവസം. (മോഡൽ വേർഷനുകൾ ട്രാക്ക് ചെയ്യാൻ).

---------------------------

Data: ഒരു മെനു കാർഡ് ഡിജിറ്റൈസ് ചെയ്തു.

LLM: Llama 3 മോഡലിനെ ഒരു സർവറിലേക്ക് (EC2) എത്തിച്ചു.

RAG: വെക്റ്റർ ഡാറ്റാബേസ് ഉപയോഗിച്ച് AI-ക്ക് റെസ്റ്റോറന്റ് വിവരങ്ങൾ പഠിപ്പിച്ചു.

Ops: MLflow ഉപയോഗിച്ച് സിസ്റ്റത്തിന്റെ പ്രകടനം ട്രാക്ക് ചെയ്തു.

UI: Streamlit ഉപയോഗിച്ച് ഉപഭോക്താക്കൾക്ക് ഉപയോഗിക്കാൻ പാകത്തിൽ ഒരു വെബ്‌സൈറ്റ് ആക്കി മാറ്റി.

-----------------------------

നിങ്ങളുടെ ഇന്നത്തെ കഠിനാധ്വാനം ഭാവിയിൽ ഉപകരിക്കുന്ന രീതിയിൽ വളരെ വ്യക്തമായി താഴെ നൽകുന്നു. ഇത് `09-march-notes.md` എന്ന പേരിൽ സേവ് ചെയ്തു വെക്കാം. ഇന്ന് നമ്മൾ നേരിട്ട വെല്ലുവിളികളും അവ മറികടന്ന രീതിയും ഇതിൽ കൃത്യമായി അടയാളപ്പെടുത്തിയിട്ടുണ്ട്.

---

# 09-March-2026: AI Assistant Project Notes

## 1. Tasks Completed Today

* **RAG System Fix:** `KeyError` പരിഹരിക്കാനായി CSV ഫയലിന് പകരം ഹാർഡ്‌കോഡ് ചെയ്ത ഡാറ്റ ഉപയോഗിച്ച് പൈത്തൺ സ്ക്രിപ്റ്റ് പരിഷ്കരിച്ചു.
* **MLflow Integration:** AI മോഡലിന്റെ പ്രകടനം (Latency, Response) ട്രാക്ക് ചെയ്യാൻ MLflow സെറ്റപ്പ് ചെയ്തു.
* **Storage Optimization:** ഡിസ്ക് സ്പേസ് കുറഞ്ഞ സാഹചര്യത്തിൽ സിസ്റ്റം ക്ലീൻ ചെയ്യാനുള്ള കമാൻഡുകൾ റൺ ചെയ്തു.
* **Streamlit UI Deployment:** ടെർമിനലിൽ നിന്ന് മാറ്റി ബ്രൗസറിൽ ഉപയോഗിക്കാവുന്ന രീതിയിൽ ഒരു വെബ് ഇന്റർഫേസ് നിർമ്മിച്ചു.

---

## 2. Main Commands Used

| Purpose | Command |
| --- | --- |
| **Run AI Bot** | `python3 ai_assistant.py` |
| **MLflow Server** | `MLFLOW_HTTP_ALLOWED_HOSTS="*" mlflow ui --host 0.0.0.0 --port 5000` |
| **Streamlit UI** | `streamlit run app.py --server.port 8501 --server.address 0.0.0.0` |
| **Clean Disk** | `pip cache purge` & `sudo apt-get clean` |
| **Check Port** | `sudo lsof -i :8501` |
| **Kill Process** | `pkill -f streamlit` or `kill -9 <PID>` |

---

## 3. Main Errors & Solutions

### Error 1: `KeyError: 'Item Name'`

* **Reason:** CSV ഫയലിലെ കോളം ഹെഡറുകൾ പൈത്തൺ സ്ക്രിപ്റ്റിലെ കോഡുമായി പൊരുത്തപ്പെട്ടില്ല.
* **Solution:** ഡാറ്റ നേരിട്ട് ഒരു ലിസ്റ്റ് ആയി കോഡിൽ നൽകി (Manual Data Injection).

### Error 2: `Invalid Host header (403 Forbidden)` in MLflow

* **Reason:** MLflow-ന്റെ പുതിയ സെക്യൂരിറ്റി ഫീച്ചർ EC2 പബ്ലിക് ഐപി വഴിയുള്ള കണക്ഷൻ ബ്ലോക്ക് ചെയ്തു.
* **Solution:** `MLFLOW_HTTP_ALLOWED_HOSTS="*"` എന്ന എൻവയോൺമെന്റ് വേരിയബിൾ സർവർ സ്റ്റാർട്ട് ചെയ്യുമ്പോൾ തന്നെ നൽകി.

### Error 3: `Port already in use` (Streamlit)

* **Reason:** മുൻപ് റൺ ചെയ്ത ഒരു സ്ട്രീംലിറ്റ് സെഷൻ ബാക്ക്ഗ്രൗണ്ടിൽ അതേ പോർട്ട് (8501) ബ്ലോക്ക് ചെയ്തിരുന്നു.
* **Solution:** `pkill -f streamlit` ഉപയോഗിച്ച് പഴയ പ്രോസസ്സുകൾ നിർത്തിയ ശേഷം റൺ ചെയ്തു.

---

## 4. Key Takeaways for Future

* **Memory Management:** EC2 ഫ്രീ ടയർ ഉപയോഗിക്കുമ്പോൾ ഇടയ്ക്കിടെ `pkill -9 python3` നൽകി മെമ്മറി ഫ്രീ ആക്കുക.
* **Security Groups:** പുതിയ സർവീസുകൾ (MLflow - 5000, Streamlit - 8501) തുടങ്ങുമ്പോൾ AWS കൺസോളിൽ ഇൻബൗണ്ട് പോർട്ടുകൾ തുറക്കാൻ മറക്കരുത്.
* **Logs:** MLflow ലോഗുകൾ `mlflow.db` എന്ന SQLite ഫയലിലേക്ക് മാറ്റുന്നത് ഡിസ്ക് സ്പേസ് ലാഭിക്കാൻ സഹായിക്കും.

---


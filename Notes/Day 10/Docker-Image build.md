കണ്ടെയ്‌നർ കാണാൻ: ബാക്ക്ഗ്രൗണ്ടിൽ റൺ ചെയ്യുന്ന കണ്ടെയ്‌നർ ലിസ്റ്റ് കാണാൻ docker ps എന്ന കമാൻഡ് ഉപയോഗിക്കാം.

ലോഗുകൾ കാണാൻ: -d മോഡിൽ റൺ ചെയ്യുമ്പോൾ ഔട്ട്‌പുട്ട് കാണാൻ കഴിയില്ല. അത് കാണാൻ docker logs <container_id> എന്ന് അടിച്ചാൽ മതി.

നിർത്താൻ: കണ്ടെയ്‌നർ ഓഫ് ചെയ്യാൻ docker stop <container_id> ഉപയോഗിക്കുക.

### Build Docker image:
bash
docker build -t alaisonbenny/ai_worker_project_api:latest .

### Run container with host network:

bash
docker run -d --network host alaisonbenny/ai_worker_project_api:latest

### Test endpoints:

bash
curl "http://localhost:8000/"

curl "http://localhost:8000/ask?q=What%20is%20in%20the%20menu%3F"


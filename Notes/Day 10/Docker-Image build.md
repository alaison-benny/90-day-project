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


## നിങ്ങൾക്ക് ഏതൊരു ടൂളും docker-compose.yaml-ലേക്ക് ആഡ് ചെയ്യാൻ ഈ ടെംപ്ലേറ്റ് ഉപയോഗിക്കാം:
Prompt Template:
"I want to add [TOOL_NAME] to my existing docker-compose.yaml file.

Current Services: [LIST_YOUR_CURRENT_SERVICES_LIKE_REDIS_API]

Network/Dependencies: It should depend on [DEPENDENCY_NAME].

Configuration: I need to mount a config file from [LOCAL_PATH] to [CONTAINER_PATH].

Purpose: I am using it for [REASON].
Please provide the YAML block with correct indentation and explain why you chose the specific image and port."

-----------------------
eg: നിങ്ങളുടെ ഈ പ്രോജക്റ്റിന് നൽകാവുന്ന കൃത്യമായ പ്രോംപ്റ്റ്:

"I want to add Grafana, Flower, and Prometheus to my docker-compose.yaml.

Current Services: Redis, Worker (Celery), and API (FastAPI).

Dependencies: Flower depends on Redis and Worker. Prometheus should monitor all.

Ports: Use standard default ports for all.

Volume: Prometheus needs a prometheus.yml config.
Please give me the code block."

-----------------------

## KW To study theory:
what are the basic concepts, main concepts, advanced concepts, ml ops concepts, main things a ML platform engineer should have clear understanding in Terraform? teach me the theory. explain me in detail, I am totally new to this, explain me with a generative AI application(ChatGPT) examples. reply me in Malayalam.

------------------------------
What are the main contents you want to add while writing a production grade in main.tf file for a generative AI application(ChatGPT)?
teach me the theory. explain me in detail, I am totally new to this. reply me in Malayalam.
------------------------------------
yes explain me Terraform Module generative AI application(ChatGPT) example
------------------------------------
Just print below given passage"
-----------------------------------
explain me below in malayalam, expalin line by line and explain in simple language "
====================
check below given repo, then tell me what is each repo about? what is end  product? what technologies and tools used in that? what are the files and folders in that repo? what is the need of each file and folder? what I want to study from each repo if I want to be a ML platform engineer? what i do reverse engineer with each folders and files for becoming a good ML platform engineer and understanding real world application and infrastrecture?"
=============
How to do do below task with this repo, I want to host this app in aws using docker, terraform, db, promethus and graffana, kuberneties, all other services and run this exactly as running in a real world production. "https://github.com/testdrivenio/fastapi-celery.git" "Asynchronous Serving: ഒരു മോഡൽ പ്രെഡിക്ഷൻ റെക്വസ്റ്റ് വരുമ്പോൾ അത് ക്യൂവിലേക്ക് (Queue) മാറ്റാനും, റെഡി ആകുമ്പോൾ റിസൾട്ട് നൽകാനും പഠിക്കണം.
Infrastructure Orchestration: Docker Compose ഉപയോഗിച്ച് ഡാറ്റാബേസും, മോഡൽ വർക്കറും എങ്ങനെ ബന്ധിപ്പിക്കാം എന്ന് പഠിക്കണം.
Scalability: എങ്ങനെ കൂടുതൽ Workers ചേർത്ത് ഒരേ സമയം നൂറുകണക്കിന് പ്രെഡിക്ഷനുകൾ നടത്താം എന്ന് മനസ്സിലാക്കാം.
Monitoring: Flower ഉപയോഗിച്ച് പരാജയപ്പെട്ട ജോലികൾ കണ്ടെത്താനും അവ വീണ്ടും റൺ ചെയ്യാനും പഠിക്കണം.
ചുമതല മാറ്റുക (Modify Task): project/worker.py-ൽ ഒരു സിമ്പിൾ പൈത്തൺ ഫംഗ്ഷന് പകരം ഒരു മെഷീൻ ലേണിംഗ് മോഡൽ (ഉദാഹരണത്തിന് Scikit-learn അല്ലെങ്കിൽ ഒരു HuggingFace മോഡൽ) ലോഡ് ചെയ്ത് നോക്കുക.
സ്കെയിലിംഗ് പരീക്ഷിക്കുക (Scale Worker): docker-compose up --scale worker=3 എന്ന് ടൈപ്പ് ചെയ്ത് മൂന്ന് വർക്കറുകൾ ഒരേ സമയം എങ്ങനെ പണി എടുക്കുന്നു എന്ന് നിരീക്ഷിക്കുക.
ബ്രോക്കർ മാറ്റുക (Change Broker): Redis-ന് പകരം RabbitMQ ഉപയോഗിക്കാൻ ശ്രമിക്കുക. ഇൻഫ്രാസ്ട്രക്ചർ എങ്ങനെ മാറുന്നു എന്ന് മനസ്സിലാക്കാം.
ലോഗിങ് (Logging): ഒരു ML മോഡൽ എറർ വരുത്തിയാൽ അത് എങ്ങനെ കണ്ടുപിടിക്കാം? docker logs ഉപയോഗിച്ച് ഓരോ വർക്കറുടെയും ലോഗുകൾ പരിശോധിക്കാൻ പഠിക്കുക.
API Security: ഈ ആപ്ലിക്കേഷനിൽ ഒരു API Key സെക്യൂരിറ്റി കൂടി ചേർക്കാൻ ശ്രമിക്കുക."
Keep in  mind I have windows 11 laptop with python, uv, docker, vs code installed, aws free trail account. explain me step by step process in detail, I am totally new to this. reply me in Malayalam.
==================

നിങ്ങൾക്ക് ഏതൊരു ടൂളും docker-compose.yaml-ലേക്ക് ആഡ് ചെയ്യാൻ ഈ ടെംപ്ലേറ്റ് ഉപയോഗിക്കാം:

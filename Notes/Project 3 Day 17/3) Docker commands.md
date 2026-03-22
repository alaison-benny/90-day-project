

1.  **പഴയ കണ്ടെയ്നർ ഡിലീറ്റ് ചെയ്യുക:**
    ```bash
    docker rm -f ml-cntainer
    ```
2.  **പുതിയ ഇമേജ് ബിൽഡ് ചെയ്യുക (v3):**
    ```bash
    docker build -t mlops-api:v3 .
    ```
3.  **കണ്ടെയ്നർ റൺ ചെയ്യുക:**
    (ഇവിടെ നമ്മൾ ഡോക്കറിനുള്ളിലെ പാത്ത് `ENV` വഴി പറഞ്ഞു കൊടുക്കുന്നു)
    ```bash
    docker run -d -p 8000:8000 \
    -e MODEL_PATH="/app/mlruns/1/models/m-553ae89e3eb44e41a87c2cbcca7bc524/artifacts" \
    --name ml-cntainer mlops-api:v3
    ```
1. എന്ത് പറ്റി എന്ന് നോക്കാം (Debugging)
കണ്ടെയ്നർ എന്തിനാണ് നിന്നതെന്ന് അറിയാൻ ഇത് അടിക്കൂ:

 ```bash
docker logs ml-cntainer
 ```

2. എല്ലാ കണ്ടെയ്നറുകളും കാണാൻ

```bash

docker ps -a
```


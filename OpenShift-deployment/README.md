# Deploying TinyLlama with vLLM on OpenShift

## 1. Log in to OpenShift
Open a terminal and log in using `oc login`:
```sh
oc login --server=<openshift-server-url> --token=<your-token>
```

> **Warning:** Ensure the OpenShift CLI (`oc`) is installed before proceeding.

## 2. Create a New Project in OpenShift
To create a new project (namespace) in OpenShift, run:
```sh
oc new-project <your-project-name>
```

> **Note:** Replace `<your-project-name>` with your desired project name.

## 3. Copy or Download Required YAML Files
Manually download or use Git to fetch the necessary YAML files:
```sh
git clone <repository-url>
```
Alternatively, download these files manually:
- `pvc.yaml`
- `vllm-deployment.yaml`
- `vllm-service.yaml`
- `vllm-route.yaml`

## 4. Update Namespace in YAML Files
Edit each file and update the `namespace` under `metadata` to reflect your project name.

## 5. Apply the YAML Files in Order
Deploy the LLM service by applying the following YAML files in sequence:
```sh
oc apply -f pvc.yaml
oc apply -f vllm-deployment.yaml
oc apply -f vllm-service.yaml
oc apply -f vllm-route.yaml
```

## 6. Access the TinyLlama LLM via API
Use the following `curl` command to send a request to the vLLM service:
```sh
curl -X 'POST' 
    'http://vllm-route-llama-infer2.apps.example.com/v1/completions' 
    -H 'accept: application/json' 
    -H 'Content-Type: application/json' 
    -d '{
        "model": "TinyLlama/TinyLlama-1.1B-Chat-v1.0",
        "prompt": "San Francisco is a",
        "max_tokens": 15,
        "temperature": 0
    }'
```

> **Note:** Replace the URL with the actual route for your deployment.

---
Following these steps, you will successfully deploy and interact with TinyLlama using vLLM on OpenShift.

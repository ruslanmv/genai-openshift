# OpenShift deployment for Gradio + Milvus + vLLM (OpenAI API) RAG UI

This directory contains the manifests used to deploy the **RAG Gradio UI**
that integrates:

- a **Milvus** vector database, and
- a **vLLM** model server exposing an OpenAI-compatible API.

The Gradio application code lives in the parent directory. This
`deployment/` folder describes the OpenShift side.

## Contents

- `configmap-collections.yaml`  
  ConfigMap defining Milvus collections and search parameters used by the
  application (for example: collection name, embedding dimension, index
  parameters).

- `configmap-prompt.yaml`  
  ConfigMap containing the system prompt or prompt template used by the RAG
  chain when sending requests to the vLLM endpoint.

- `deployment.yaml`  
  Deployment for the Gradio RAG UI pod. It typically includes:

  - image reference for the UI,
  - environment variables for Milvus (host, port, username/password, TLS),
  - environment variables for vLLM (OpenAI base URL, API key if required,
    model name),
  - resource requests/limits and labels.

- `service.yaml`  
  ClusterIP Service for the UI.

- `route.yaml`  
  OpenShift Route exposing the UI to end users.

## Prerequisites

- A running **Milvus** instance with collections populated with document
  embeddings.
- A **vLLM** server exposing an OpenAI-style API.
- An OpenShift namespace and `oc` access.

## Deployment steps

1. Review and customise the ConfigMaps:

   - `configmap-collections.yaml`: match collection names and Milvus
     configuration to your setup.
   - `configmap-prompt.yaml`: tune the system prompt / template as needed.

2. Edit `deployment.yaml` to:

   - point to the correct container image,
   - configure Milvus and vLLM connection settings via environment
     variables or configuration files,
   - adjust resources if necessary.

3. Adjust `route.yaml` with a suitable `spec.host` value for your cluster
   (or rely on generated hosts if using default wildcard routing).

4. Apply all manifests:

   ```bash
   oc project genai-demo

   oc apply -f configmap-collections.yaml
   oc apply -f configmap-prompt.yaml
   oc apply -f deployment.yaml
   oc apply -f service.yaml
   oc apply -f route.yaml
   ```

5. Verify the pod is running and the Route is available:

   ```bash
   oc get pods
   oc get route
   ```

6. Open the Route URL in a browser to use the RAG UI.

## Cleaning up

```bash
oc delete -f route.yaml
oc delete -f service.yaml
oc delete -f deployment.yaml
oc delete -f configmap-prompt.yaml
oc delete -f configmap-collections.yaml
```

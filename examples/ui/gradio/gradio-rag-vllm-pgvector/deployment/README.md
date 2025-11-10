# OpenShift deployment for Gradio + vLLM + PostgreSQL/pgvector RAG UI

This directory holds the manifests to deploy the **Gradio RAG UI** that
uses:

- a **vLLM** server exposing an OpenAI-compatible API, and
- a **PostgreSQL/pgvector** database for vector search.

The application code is located one level up; these manifests cover the
deployment to OpenShift.

## Contents

- `deployment.yaml`  
  Deployment for the Gradio RAG UI pod. It typically sets:

  - container image for the UI,
  - connection parameters for the vLLM endpoint,
  - PostgreSQL/pgvector connection settings,
  - resource requests/limits and labels.

- `service.yaml`  
  ClusterIP Service for in-cluster traffic.

- `route.yaml`  
  Route exposing the UI to external users.

## Prerequisites

- A running **vLLM** instance exposing an OpenAI-style API.
- A **PostgreSQL** database with `pgvector` enabled and populated with
  embeddings.
- An OpenShift namespace and `oc` access.

## Deployment steps

1. Edit `deployment.yaml` to:

   - specify the Gradio UI image,
   - set environment variables for:
     - vLLM base URL / model name,
     - PostgreSQL host, port, database, user and password,
     - vector table/index information.

2. Optionally update `route.yaml` with your preferred host name.

3. Apply the manifests:

   ```bash
   oc project genai-demo

   oc apply -f deployment.yaml
   oc apply -f service.yaml
   oc apply -f route.yaml
   ```

4. Confirm that the pod is running:

   ```bash
   oc get pods
   ```

5. Retrieve the Route URL and open the UI:

   ```bash
   oc get route
   ```

## Cleaning up

```bash
oc delete -f route.yaml
oc delete -f service.yaml
oc delete -f deployment.yaml
```

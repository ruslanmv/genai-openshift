# OpenShift deployment for Gradio + TGI + PostgreSQL/pgvector RAG UI

This directory contains the manifests required to deploy the **RAG Gradio
UI** that combines:

- a **Hugging Face TGI** text-generation server, and
- a **PostgreSQL database with pgvector** as the vector store.

The UI code lives one directory above; here we only describe the
OpenShift/Kubernetes resources.

## Contents

- `deployment.yaml`  
  Deployment for the Gradio RAG UI pod. It typically configures:

  - the Gradio container image,
  - connection settings for the TGI endpoint,
  - connection settings for PostgreSQL/pgvector (host, port, database,
    credentials, table/index names),
  - resource requests/limits and labels.

- `service.yaml`  
  ClusterIP Service for in-cluster access to the UI.

- `route.yaml`  
  Route exposing the UI externally to users.

## Prerequisites

- A running **TGI server** reachable from the cluster.
- A running **PostgreSQL instance** with the `pgvector` extension enabled,
  populated with documents and embeddings for RAG.
- An OpenShift project/namespace for the deployment.

## Deployment steps

1. Edit `deployment.yaml`:

   - Set the correct image reference for the Gradio UI.
   - Provide environment variables for:
     - TGI endpoint URL,
     - PostgreSQL connection (`PGHOST`, `PGPORT`, `PGUSER`, `PGPASSWORD`,
       `PGDATABASE`, etc.),
     - vector table/index names.

2. Optionally customise `route.yaml` with the appropriate `spec.host` for
   your cluster.

3. Apply the manifests:

   ```bash
   oc project genai-demo   # or your chosen namespace

   oc apply -f deployment.yaml
   oc apply -f service.yaml
   oc apply -f route.yaml
   ```

4. Confirm that the pod is running:

   ```bash
   oc get pods
   ```

5. Retrieve the Route URL and use it to access the Gradio UI:

   ```bash
   oc get route
   ```

## Cleaning up

```bash
oc delete -f route.yaml
oc delete -f service.yaml
oc delete -f deployment.yaml
```

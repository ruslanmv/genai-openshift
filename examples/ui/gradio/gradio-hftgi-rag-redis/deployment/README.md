# OpenShift deployment for Gradio + TGI + Redis RAG UI

This directory holds the manifests used to deploy the **RAG Gradio UI**
that uses:

- **Hugging Face TGI** for text generation, and
- **Redis** as a vector database (e.g. using RediSearch).

The Python code for the UI lives in the parent directory. This README
focuses on how to deploy it on OpenShift.

## Contents

- `cm_redis_schema.yaml`  
  ConfigMap containing the Redis index schema used by the RAG pipeline.
  It is loaded by the application at startup so that it can query the
  vector index consistently.

- `deployment.yaml`  
  Deployment manifest for the Gradio UI. It configures:

  - image reference,
  - TGI endpoint address,
  - Redis connection settings (host, port, database, password, index name),
  - resource requests/limits and labels.

- `service.yaml`  
  ClusterIP Service providing in-cluster access to the UI.

- `route.yaml`  
  OpenShift Route exposing the Service externally.

## Prerequisites

- A running **TGI server**.
- A **Redis** instance with a vector index already populated.
- An OpenShift namespace and access via `oc`.

## Deployment steps

1. Review `cm_redis_schema.yaml` and adapt the schema if your Redis setup
   uses different field names or index configuration.

2. Edit `deployment.yaml`:

   - adjust the container image reference,
   - configure environment variables for:
     - TGI URL,
     - Redis connection and index name,
     - any app-specific settings (temperature, top_p, etc.).

3. Optionally update `route.yaml` with your desired public host name.

4. Apply the manifests:

   ```bash
   oc project genai-demo

   oc apply -f cm_redis_schema.yaml
   oc apply -f deployment.yaml
   oc apply -f service.yaml
   oc apply -f route.yaml
   ```

5. Check that the pod is running and ready:

   ```bash
   oc get pods
   ```

6. Get the Route URL and open the UI in a browser:

   ```bash
   oc get route
   ```

## Cleaning up

```bash
oc delete -f route.yaml
oc delete -f service.yaml
oc delete -f deployment.yaml
oc delete -f cm_redis_schema.yaml
```

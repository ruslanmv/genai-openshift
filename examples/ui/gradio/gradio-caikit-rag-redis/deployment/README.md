# OpenShift deployment for Gradio Caikit + Redis RAG UI

This directory contains the OpenShift/Kubernetes manifests used to deploy
the **Gradio RAG UI** that talks to:

- a **Caikit + TGIS** LLM endpoint, and
- a **Redis** instance used as a vector database.

The parent directory (one level up) contains the Python application code and a
more general description of the demo UI. This `deployment/` folder focuses on
how to run that UI on an OpenShift cluster.

## Contents

- `cm_certificate.yaml`  
  ConfigMap that embeds the certificate used to connect securely to the
  Caikit/TGIS endpoint (for example when it is exposed over HTTPS with a
  custom CA).

- `cm_redis_schema.yaml`  
  ConfigMap that defines the Redis index / schema used by the RAG pipeline.
  It is mounted into the Gradio pod so that the UI knows how to query the
  vector index.

- `deployment.yaml`  
  Deployment for the Gradio RAG UI pod. It sets:

  - the container image,
  - environment variables such as the Caikit/TGIS endpoint URL and the Redis
    connection parameters,
  - resource requests/limits,
  - labels used by the Service and Route.

- `service.yaml`  
  ClusterIP Service that exposes the Gradio pod internally in the cluster.

- `route.yaml`  
  OpenShift Route that exposes the Service to users (HTTP or HTTPS). The
  host name is usually customised per cluster.

## Prerequisites

Before deploying these manifests you should have:

1. A running **Caikit + TGIS** deployment with an LLM loaded and reachable
   from the OpenShift cluster.
2. A running **Redis** instance configured as a vector database and already
   populated with documents and embeddings.
3. An OpenShift project/namespace where you want to deploy this UI.
4. The `oc` CLI configured to talk to your cluster.

## Deployment steps

1. Review the configuration in the manifests:

   - Edit `cm_certificate.yaml` if you need to inject your own certificate.
   - Edit `cm_redis_schema.yaml` only if your Redis index name or schema
     differs from the default.
   - Edit `deployment.yaml` to adjust:
     - image reference (registry, tag),
     - environment variables (Caikit/TGIS URL, Redis host/port/password,
       index name, etc.),
     - resource requests/limits if needed.
   - Edit `route.yaml` to set the appropriate `spec.host` for your cluster.

2. Apply the manifests to your target namespace (example: `genai-demo`):

   ```bash
   oc project genai-demo

   oc apply -f cm_certificate.yaml
   oc apply -f cm_redis_schema.yaml
   oc apply -f deployment.yaml
   oc apply -f service.yaml
   oc apply -f route.yaml
   ```

3. Wait for the Deployment to become ready:

   ```bash
   oc get pods
   ```

   You should see a pod with labels `app=gradio-caikit-rag-redis` (or the
   label defined in the Deployment) in `Running` state.

4. Retrieve the Route URL and open it in a browser:

   ```bash
   oc get route
   ```

   Use the `HOST/PORT` column to access the Gradio UI.

## Cleaning up

To remove this deployment from the namespace:

```bash
oc delete -f route.yaml
oc delete -f service.yaml
oc delete -f deployment.yaml
oc delete -f cm_redis_schema.yaml
oc delete -f cm_certificate.yaml
```

This only affects the Gradio UI; the Caikit/TGIS and Redis deployments are
managed separately.

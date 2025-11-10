# Standalone deployment of Docling Server on OpenShift

This directory contains plain Kubernetes/OpenShift manifests to deploy
**Docling Server** as a regular application (outside of the model-serving
stack).

## Contents

- `deployment.yaml`  
  Deployment for the Docling Server pod. It runs the image built from
  `tools/docling-server/container/` and typically mounts the model and
  OCR artefacts prepared by `models-download.py`.

- `service.yaml`  
  ClusterIP Service exposing the HTTP API inside the cluster.

- `route.yaml`  
  Route exposing the Service externally to clients.

## Prerequisites

- A Docling Server image built and pushed to a registry reachable from the
  cluster.
- An OpenShift namespace in which to deploy the service.

## Deployment steps

1. Edit `deployment.yaml` and set:

   - the `image:` reference to your Docling Server image,
   - any required environment variables,
   - volume mounts (if you store models or configuration on a PVC),
   - resource requests/limits.

2. Optionally adjust `route.yaml` to configure the public host name.

3. Apply the manifests:

   ```bash
   oc project genai-docs   # example namespace

   oc apply -f deployment.yaml
   oc apply -f service.yaml
   oc apply -f route.yaml
   ```

4. Verify that the pod is running and the Route is available:

   ```bash
   oc get pods
   oc get route
   ```

5. Access the Docling Server API using the Route URL (for example,
   `https://docling-server.apps.<cluster-domain>`).

## Cleaning up

```bash
oc delete -f route.yaml
oc delete -f service.yaml
oc delete -f deployment.yaml
```

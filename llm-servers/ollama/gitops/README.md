# GitOps deployment for Ollama on OpenShift

This directory provides a GitOps layout for running **Ollama** on
OpenShift, including storage, networking and (optionally) Argo CD
integration.

## Contents

- `ollama-app.yaml`  
  Argo CD `Application` resource pointing at this folder. It specifies the
  Git repository, path and target namespace.

- `kustomization.yaml`  
  Kustomize configuration referencing the deployment, service, route and
  PVC manifests.

- `deployment.yaml`  
  Deployment running the Ollama server container. You can adjust:

  - image reference,
  - GPU/CPU resources,
  - environment variables (such as model pull behaviour, cache paths),
  - volume mounts for model storage.

- `pvc.yaml`  
  PersistentVolumeClaim providing storage for Ollama models.

- `service.yaml`  
  Service exposing the Ollama HTTP endpoint inside the cluster.

- `route.yaml`  
  Route exposing the Ollama service externally.

## Usage with Argo CD

1. Commit this directory to a Git repo accessible from Argo CD.
2. Update `ollama-app.yaml` with the correct repo URL, path and namespace.
3. Apply the application resource:

   ```bash
   oc apply -f ollama-app.yaml
   ```

4. Synchronise the application from the Argo CD UI.

## Usage with Kustomize only

To deploy without Argo CD:

```bash
oc project genai-demo   # or your namespace

oc apply -k .
```

This will create the PVC, Deployment, Service and Route that together expose
an Ollama server on OpenShift.

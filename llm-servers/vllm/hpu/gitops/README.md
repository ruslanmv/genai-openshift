# GitOps deployment for vLLM (HPU) on OpenShift

This directory contains a GitOps-style layout for deploying the
**vLLM** model server on OpenShift, targeting the `hpu`
compute flavour.

The manifests are structured so they can be consumed either directly
with Kustomize or via a GitOps controller such as Argo CD.

## Contents

- `vllm-app.yaml`  
  Argo CD `Application` resource pointing at this folder. It specifies
  the Git repository, path and target namespace for vLLM.

- `kustomization.yaml`  
  Kustomize configuration referencing the deployment, PVC, Service and
  Route.

- `deployment.yaml`  
  Deployment running the vLLM server container. You should customise:

  - container image (CPU/GPU/HPU appropriate),
  - model name and weights location,
  - resource requests/limits and scheduling hints,
  - environment variables (such as tensor parallelism, dtype, etc.).

- `pvc.yaml`  
  PersistentVolumeClaim used to store model artefacts for vLLM.

- `service.yaml`  
  Service exposing the vLLM HTTP endpoint inside the cluster.

- `route.yaml`  
  Route exposing the vLLM service externally.

## Usage with Argo CD

1. Push this directory to a Git repository accessible by Argo CD.
2. Update `vllm-app.yaml` with the correct repo URL, path and namespace.
3. Apply the Application resource:

   ```bash
   oc apply -f vllm-app.yaml
   ```

4. Synchronise the application in the Argo CD UI.

## Usage with Kustomize only

To deploy without Argo CD:

```bash
oc project genai-demo   # or your namespace

oc apply -k .
```

This will create the PVC, Deployment, Service and Route that expose
vLLM for this `hpu` configuration.

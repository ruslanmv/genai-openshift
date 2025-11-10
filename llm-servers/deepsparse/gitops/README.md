# GitOps deployment for DeepSparse text generation server

This directory contains a GitOps-friendly layout for deploying the
**DeepSparse** text-generation server on OpenShift, typically managed by
Argo CD or a similar GitOps tool.

## Contents

- `deepsparse-app.yaml`  
  Argo CD `Application` resource pointing to the manifests in this folder.
  It describes:

  - the target namespace,
  - the Git repository and path,
  - sync policy (manual/automatic),
  - and other GitOps metadata.

- `kustomization.yaml`  
  Kustomize configuration that groups the deployment resources below.
  It allows Argo CD (or the `oc kustomize` / `kubectl kustomize` commands)
  to apply the resources as a single unit.

- `deployment.yaml`  
  Deployment for the DeepSparse text-generation server pod. Adjust this
  file to set the container image, model path, resources, and environment
  variables.

- `pvc.yaml`  
  PersistentVolumeClaim for storing model artefacts or other data required
  by DeepSparse.

- `service.yaml`  
  ClusterIP Service exposing the server inside the cluster.

- `route.yaml`  
  Route exposing the Service to external clients.

## Using with Argo CD

1. Commit this directory to a Git repository accessible by your Argo CD
   instance.
2. Update `deepsparse-app.yaml` to reference the correct repository URL,
   path, and target namespace.
3. Apply the Application to the Argo CD control plane:

   ```bash
   oc apply -f deepsparse-app.yaml
   ```

4. In the Argo CD UI, sync the application and watch the resources being
   created.

## Using kustomize directly

If you are not using Argo CD, you can apply these resources directly with
`oc` and Kustomize:

```bash
oc project genai-demo   # or your namespace

oc apply -k .
```

This will create the PVC, Deployment, Service and Route defined in the
kustomization.

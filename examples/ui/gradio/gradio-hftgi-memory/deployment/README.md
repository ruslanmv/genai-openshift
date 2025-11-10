# OpenShift deployment for Gradio + Hugging Face TGI (with memory)

This directory contains the OpenShift manifests required to deploy the
**Gradio chat UI** that connects to a **Hugging Face Text Generation
Inference (TGI)** server and maintains basic conversation memory.

The parent directory contains the Gradio application code. This
`deployment/` folder focuses purely on the Kubernetes/OpenShift resources.

## Contents

- `deployment.yaml`  
  Deployment for the Gradio UI pod. It typically sets:

  - container image for the Gradio app,
  - environment variables (TGI endpoint URL, model name, timeouts, etc.),
  - resource requests/limits,
  - labels used by the Service and Route.

- `service.yaml`  
  ClusterIP Service exposing the Gradio pod inside the cluster.

- `route.yaml`  
  OpenShift Route exposing the Service to users over HTTP/HTTPS.

## Prerequisites

- A running **Hugging Face TGI** endpoint accessible from the cluster.
- An OpenShift project/namespace for the demo.
- `oc` CLI configured to talk to the cluster.

## Deployment steps

1. Review `deployment.yaml`:

   - Configure the TGI endpoint URL (e.g. via environment variables).
   - Adjust the container image reference if you built and pushed a custom
     image.
   - Optionally tune CPU/memory requests and limits.

2. Review `route.yaml` and adapt the `spec.host` to your cluster domain if
   necessary.

3. Apply the manifests (example namespace: `genai-demo`):

   ```bash
   oc project genai-demo

   oc apply -f deployment.yaml
   oc apply -f service.yaml
   oc apply -f route.yaml
   ```

4. Wait for the pod to be created and become ready:

   ```bash
   oc get pods
   ```

5. Retrieve the Route URL and open the Gradio UI in your browser:

   ```bash
   oc get route
   ```

## Cleaning up

```bash
oc delete -f route.yaml
oc delete -f service.yaml
oc delete -f deployment.yaml
```

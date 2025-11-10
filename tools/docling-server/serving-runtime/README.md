# Serving Runtime for Docling Server (OpenShift AI / ODH)

This directory provides a `ServingRuntime` / `InferenceService` manifest
that integrates **Docling Server** into the Single-Model Serving stack of
Open Data Hub or Red Hat OpenShift AI.

## Contents

- `servingruntime_inferenceservice.yaml`  
  Composite manifest that defines:

  - the `ServingRuntime` describing the Docling Server container image and
    supported protocol,
  - (optionally) an `InferenceService` that uses this runtime.

## Usage

1. Ensure that you have a Docling Server image available in a registry
   accessible by your cluster (see `tools/docling-server/container/` for
   container build files).

2. Edit `servingruntime_inferenceservice.yaml` and adjust:

   - image reference,
   - resource requests/limits,
   - any environment variables or volumes that your Docling deployment
     requires.

3. Apply the manifest in a data science / model-serving enabled namespace:

   ```bash
   oc project rhods-notebooks   # or your ODH/OpenShift AI project

   oc apply -f servingruntime_inferenceservice.yaml
   ```

4. In the OpenShift AI (RHODS) dashboard, the runtime should appear as an
   available Serving Runtime, and the InferenceService (if defined) should
   be visible under “Model Serving”.

Consult the top-level Docling Server README and the OpenShift AI
documentation for details on how to bind PVCs and expose routes through
the model-serving stack.

# SentenceTransformers embedding server (GPU)

This directory mirrors the CPU variant but targets **GPU-enabled nodes**,
for higher throughput embedding generation on OpenShift.

## Contents

- `Containerfile`  
  Container build recipe for the GPU embedding server. It typically uses a
  CUDA-enabled base image and installs the same application code as the CPU
  variant.

- `app.py`  
  Same FastAPI application as in `llm-servers/sbert/cpu/app.py`. It loads
  the model using SentenceTransformers and exposes `/health` and
  `/v1/embeddings` endpoints.

- `requirements.txt`, `Pipfile`, `Pipfile.lock`  
  Dependency definitions, similar to the CPU variant.

- `cuda.repo-x86_64`, `LICENSE`, `NGC-DL-CONTAINER-LICENSE`  
  Additional files used for the NVIDIA CUDA repository and container
  licensing information.

## GPU-specific considerations

- Ensure that your OpenShift cluster has GPU-enabled worker nodes and that
  the appropriate device plugin is installed.
- In the Deployment that uses this image, set:
  - `resources.limits.nvidia.com/gpu: 1` (or more),
  - node selectors or tolerations matching your GPU nodes.

The application code itself does not change; SentenceTransformers will use
the available CUDA device when running inside the GPU-enabled container.

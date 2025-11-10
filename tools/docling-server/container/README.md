# Container definitions for Docling Server

This directory contains container build files and helper scripts to create
**Docling Server** images for CPU and CUDA-enabled environments.

These images wrap the Docling CLI and the HTTP API defined in
`tools/docling-server/container/common/app.py`.

## Contents

- `Containerfile.cpu`  
  Build recipe for the CPU-only Docling Server image. Typically:

  - installs required OS packages (see `common/os-packages.txt`),
  - installs Python dependencies (Docling, FastAPI, Uvicorn),
  - copies the `common/` Python code,
  - sets the default command to run the FastAPI application.

- `Containerfile.cuda`  
  Build recipe for a CUDA-enabled Docling Server image. It uses a GPU base
  image, installs additional CUDA dependencies and then configures the
  same Python application as the CPU image.

- `startdev-cpu.sh`, `startdev-cuda.sh`  
  Convenience scripts for local development. They usually:

  - build and run the corresponding image,
  - mount a local directory for models or test documents,
  - expose the FastAPI service on a local port for testing.

## Relationship with `common/`

The `common/` directory (one level down) contains:

- `app.py` — the FastAPI application exposing Docling as a service,
- `models-download.py` — utility to pre-download Docling models and OCR
  artefacts,
- `os-packages.txt` — list of OS packages required at build time.

Both container files copy these resources and use them as the basis of the
image.

## Building images

Example build commands (adjust registry and tags as appropriate):

```bash
podman build -f Containerfile.cpu -t quay.io/ORG/docling-server-cpu .
podman build -f Containerfile.cuda -t quay.io/ORG/docling-server-cuda .
```

Once built and pushed, you can reference these images from your OpenShift
Deployments or Serving Runtimes.

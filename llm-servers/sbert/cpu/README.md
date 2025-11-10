# SentenceTransformers embedding server (CPU)

This directory defines a **CPU-based embedding microservice** built with
FastAPI and [SentenceTransformers](https://www.sbert.net/). The service
exposes an OpenAI-compatible `/v1/embeddings` endpoint and is designed to
run on OpenShift.

## Contents

- `Containerfile`  
  Container build recipe for the CPU variant of the embedding server.
  It typically:

  - installs Python dependencies,
  - copies `app.py` into the image,
  - configures the default model path.

- `requirements.txt`  
  Python dependencies required at runtime (FastAPI, transformers,
  sentence-transformers, etc.).

- `Pipfile`, `Pipfile.lock`  
  Optional Pipenv configuration for local development.

- `app.py`  
  FastAPI application that:

  - loads the SentenceTransformers model from `--model-path`,
  - defines Pydantic models `EmbeddingRequest`, `EmbeddingObject` and
    `EmbeddingResponse`,
  - implements:
    - `GET /health` — basic health probe,
    - `POST /v1/embeddings` — OpenAI-style embeddings endpoint.

  The `create_embeddings` handler:

  1. Normalises the `input` field to a list of strings.
  2. Encodes each input with a tokenizer to compute token usage.
  3. Calls the SentenceTransformers model to obtain embeddings.
  4. Returns an OpenAI-compatible response object.

## Running locally

1. Create a virtual environment and install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

2. Download or mount a SentenceTransformers model under `/mnt/models` or
   pass a custom `--model-path`:

   ```bash
   python app.py --model-path /mnt/models
   ```

   By default the script will start a Uvicorn server on port `8080`.

3. Call the embeddings endpoint:

   ```bash
   curl -X POST http://localhost:8080/v1/embeddings          -H "Content-Type: application/json"          -d '{"input": "hello world", "model": "local-sbert"}'
   ```

## Building and running on OpenShift

1. Build and push the image (example using OpenShift Builds or `podman`):

   ```bash
   podman build -t quay.io/ORG/sbert-embed-cpu -f Containerfile .
   podman push quay.io/ORG/sbert-embed-cpu
   ```

2. Create a Deployment and Service that use the built image (see other
   directories in this repo for sample manifests).

3. Mount the model files into the container at the path specified by
   `--model-path` (for example using a PersistentVolumeClaim).

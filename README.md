<p align="center">
  <img src="assets/banner.svg" width="100%" alt="GenAI on OpenShift banner">
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache_2.0-7a0000.svg" alt="License: Apache-2.0"></a>
  <img src="https://img.shields.io/badge/Platform-Red%20Hat%20OpenShift-informational.svg" alt="Platform: OpenShift">
  <img src="https://img.shields.io/badge/Focus-GenAI%20%7C%20RAG%20%7C%20Doc%20Understanding-lightgrey.svg" alt="Focus">
</p>

# GenAI on OpenShift (unofficial toolkit)

This repository contains resources, demos, and recipes for building and operating GenAI workloads — including **LLMs**, **RAG pipelines**, **vector stores**, and **document understanding** — on **Red Hat OpenShift AI** (formerly RHODS) and **Open Data Hub**.

---

## Overview

This repository is organised around the core components of a modern GenAI stack for OpenShift:

<table>
  <tr>
    <td width="80"><img src="assets/inference.svg" alt="Inference servers icon"></td>
    <td><strong>Inference servers for LLMs and embeddings.</strong></td>
  </tr>
  <tr>
    <td><img src="assets/runtimes.svg" alt="Serving runtimes icon"></td>
    <td><strong>Serving Runtimes</strong> for OpenShift AI / ODH Single-Model Serving.</td>
  </tr>
  <tr>
    <td><img src="assets/vectordb.svg" alt="Vector DB icon"></td>
    <td><strong>Vector databases</strong> for Retrieval-Augmented Generation (RAG).</td>
  </tr>
  <tr>
    <td><img src="assets/examples.svg" alt="Examples icon"></td>
    <td><strong>Inference and application examples</strong> (notebooks and UIs).</td>
  </tr>
  <tr>
    <td><img src="assets/clients.svg" alt="Clients icon"></td>
    <td><strong>LLM clients</strong> that connect to your OpenShift-hosted endpoints.</td>
  </tr>
</table>

---

## Inference Servers

The following inference servers can be deployed standalone on OpenShift:

- <strong><a href="llm-servers/vllm/gpu/README.md">vLLM</a></strong> — how to deploy <a href="https://docs.vllm.ai/en/latest/index.html">vLLM</a>, “Easy, fast, and cheap LLM serving for everyone.”
- <strong><a href="llm-servers/hf_tgi/README.md">Hugging Face TGI</a></strong> — how to deploy the <a href="https://github.com/huggingface/text-generation-inference">Text Generation Inference</a> server.
- <strong><a href="https://github.com/opendatahub-io/caikit-tgis-serving">Caikit‑TGIS‑Serving</a></strong> (external) — how to deploy the Caikit‑TGIS‑Serving stack from Open Data Hub.
- <strong><a href="llm-servers/ollama/README.md">Ollama</a></strong> — how to deploy <a href="https://github.com/ollama/ollama">Ollama</a> using CPU‑only for inference.
- <strong><a href="llm-servers/sbert/README.md">SBERT</a></strong> — runtime to serve <a href="https://huggingface.co/sentence-transformers">Sentence Transformers</a> models for embeddings.

---

## Serving Runtimes (OpenShift AI / ODH)

Import these runtime definitions into the **Single‑Model Serving** stack of Open Data Hub or OpenShift AI:

- <strong><a href="serving-runtimes/vllm_runtime/README.md">vLLM Serving Runtime</a></strong>
- <strong><a href="serving-runtimes/hf_tgi_runtime/README.md">Hugging Face TGI Runtime</a></strong>
- <strong><a href="serving-runtimes/sbert_runtime/README.md">SBERT Runtime</a></strong>
- <strong><a href="serving-runtimes/ollama_runtime/ollama-runtime.yaml">Ollama Runtime</a></strong>

---

## Vector Databases

Use these databases as vector stores for **RAG** applications:

- <strong><a href="vector-databases/milvus/README.md">Milvus</a></strong> — full recipe to deploy Milvus in standalone or cluster mode.
- <strong><a href="vector-databases/pgvector/README.md">PostgreSQL + pgvector</a></strong> — full recipe to create a PostgreSQL instance with the `pgvector` extension.
- <strong><a href="vector-databases/redis/README.md">Redis</a></strong> — full recipe to deploy Redis, create a cluster, and configure a vector‑store database.

---

## Inference and Application Examples

- <strong><a href="examples/notebooks/caikit-basic-query/README.md">Caikit</a></strong> — basic example using Caikit + TGIS for LLM serving.
- <strong><a href="examples/notebooks/langchain/README.md">LangChain examples</a></strong> — notebooks showcasing patterns with different LLM servers and vector databases.
- <strong><a href="examples/langflow/README.md">Langflow examples</a></strong> — build flows visually on top of OpenShift AI infrastructure.
- <strong><a href="examples/ui/README.md">UI examples</a></strong> — Gradio‑based UIs and frontends for interacting with LLMs and RAG pipelines.

---

## LLM Clients

- <strong><a href="llm-clients/anythingllm/Readme.md">AnythingLLM</a></strong> — an all‑in‑one AI application (any LLM, any document, any agent, fully private). Implemented here as a **RHOAI custom workbench** that connects to your LLM endpoints and vector databases.

---

## How to use this toolkit

1. Select an **inference server** (vLLM, TGI, Caikit‑TGIS, Ollama, SBERT).
2. Import the relevant **Serving Runtime** in OpenShift AI / ODH.
3. Deploy a **vector database** (Milvus, PostgreSQL+pgvector, or Redis) for RAG.
4. Use the provided **notebooks and UI examples** to compose full GenAI workflows.

---

## Contributing

This is an **unofficial** toolkit. Issues, suggestions, and pull requests are welcome.

Licensed under the Apache License, Version 2.0. See <a href="LICENSE">LICENSE</a> for details.

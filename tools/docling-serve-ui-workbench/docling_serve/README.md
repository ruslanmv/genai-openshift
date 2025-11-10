# Docling Serve UI workbench package

This directory contains the Python package used by the **Docling Serve
client workbench** image. It bundles a FastAPI backend and a Gradio-based
web UI that connect to a Docling Serve instance for document conversion
and exploration.

The parent directory (`tools/docling-serve-ui-workbench/`) contains a
higher-level README focusing on the container image and workbench usage.
This README explains the structure of the `docling_serve` package itself.

## Package structure

- `__init__.py`  
  Marks this directory as a Python package.

- `__main__.py`  
  Typer-based command line entry point. Exposes commands such as:

  - `run` / `dev` — start the FastAPI + Gradio application,
  - `version` — print version information,
  - configuration flags mirroring `UvicornSettings` and `DoclingServeSettings`.

- `app.py`  
  FastAPI application factory and HTTP endpoints. It is responsible for:

  - creating the main FastAPI `app` instance,
  - configuring CORS and static file serving,
  - wiring a simple notebook-like API used by the UI,
  - exposing health endpoints and a root redirect to the UI.

- `gradio_ui.py`  
  Definition of the Gradio interface. It builds a multi-tab UI that allows
  users to:

  - upload documents or specify URLs,
  - trigger processing via Docling Serve,
  - inspect the extracted Markdown/JSON/HTML representation.

  The module also includes helpers to toggle output visibility and control
  download behaviour.

- `helper_functions.py`  
  Utility functions used both by the FastAPI layer and by the Gradio
  components, for example:

  - `FormDepends` and `as_form_func` — helpers for building FastAPI form
    dependencies from Pydantic models,
  - `_to_list_of_strings`, `split_and_strip` — helpers to normalise various
    input formats.

- `settings.py`  
  Pydantic settings classes:

  - `UvicornSettings` — host/port, reload, workers, root path, etc.,
  - `DoclingServeSettings` — feature flags and configuration for the
    workbench itself (UI enablement, artefact paths, last activity).

- `static/`  
  Static assets (HTML, JavaScript, CSS) used by the web UI.

## How the pieces fit together

1. `__main__.py` parses CLI arguments and constructs `UvicornSettings` and
   `DoclingServeSettings` from environment variables and flags.
2. It instantiates the FastAPI application defined in `app.py`.
3. `app.py` mounts the Gradio interface built in `gradio_ui.py` under a
   certain path and serves the static assets from `static/`.
4. When a user uploads a document or URL via the Gradio UI, the backend
   calls a remote Docling Serve endpoint, processes the response and
   updates the UI outputs.

To understand the request/response flow step by step, start from
`__main__.py` (entrypoint), then follow into `app.py` (FastAPI app) and
finally `gradio_ui.py` (UI and callbacks).

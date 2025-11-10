# Common FastAPI application for Docling Server containers

This directory holds the Python application code and auxiliary files shared
by the CPU and CUDA Docling Server container images.

## Contents

- `app.py`  
  FastAPI application wrapping the Docling CLI. It exposes endpoints to:

  - check health (`GET /health`),
  - process documents from a URL (`POST /process_url`),
  - process uploaded files (`POST /process_file`).

  The core helper `docling_processing`:

  1. Writes input sources (files or URLs) into a temporary directory.
  2. Invokes the Docling CLI with parameters derived from the request
     (formats, OCR options, backends, etc.).
  3. Packages the output directory into a single file when appropriate
     (for example, a `.zip` when multiple artefacts are produced).
  4. Returns the result as a streamed `FileResponse` and attaches
     background tasks to clean up temporary files.

  The Pydantic models `DoclingBaseParameters` and `DoclingParameters`
  describe all the conversion options exposed by the API.

- `models-download.py`  
  Utility script executed at image build time or container startup to:

  - download the Docling model snapshot from Hugging Face,
  - download language packs for EasyOCR,
  - extract them into the expected model directories.

  This ensures that the container has all required artefacts available
  locally before the first request.

- `os-packages.txt`  
  List of OS packages required by the Docling toolchain (for example
  `tesseract`, Ghostscript, fonts, etc.). The container build process uses
  this list in its package installation step.

## Flow overview

1. A client calls `/process_url` or `/process_file` with the desired
   formats and options.
2. `DoclingParameters` is constructed from the request body / form data.
3. `docling_processing` invokes the Docling CLI and collects outputs.
4. The FastAPI route returns either a single converted file or a zip
   archive containing multiple artefacts.

This logic is shared by both CPU and CUDA images through the `common/`
directory.

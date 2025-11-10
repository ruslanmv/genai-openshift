# NGINX container scripts for AnythingLLM

This directory contains helper scripts used by the NGINX container image
that fronts the **AnythingLLM** workbench.

In OpenShift-based images, it is common to ship generic container scripts
under `/usr/share/container-scripts/nginx` to help extend or customise
the NGINX configuration.

## Contents

- `common.sh`  
  Shell helpers to:

  - discover additional configuration fragments mounted into the container,
  - process and include those fragments in the final NGINX configuration.

  The functions in this script are typically sourced by the image's
  entrypoint or wrapper scripts so that you can drop extra `.conf` files in
  well-known directories (for example via ConfigMaps) without modifying the
  base image.

## How it is used

At build time, this file is copied into the container image. At runtime,
the NGINX startup logic calls the functions defined here to:

1. Look for override or extension configuration files in a “custom”
   directory.
2. Merge them with the default configuration shipped in the image.
3. Start NGINX with the resulting configuration.

If you need to extend the reverse proxy behaviour for AnythingLLM (for
example to add extra headers or tweak timeouts), you can:

1. Create a ConfigMap holding additional `.conf` files.
2. Mount it into the path expected by `common.sh` as the “custom”
   directory.
3. Let the container scripts merge your fragments with the defaults.

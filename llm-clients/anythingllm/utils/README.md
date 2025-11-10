# Utility scripts for AnythingLLM workbench

This directory provides small utility scripts that are used by the
**AnythingLLM** custom workbench image when running on OpenShift.

## Contents

- `process.sh`  
  Bash helper to start a child process and handle termination cleanly.

  The script:

  1. Starts the command passed as arguments and remembers its PID.
  2. Installs signal handlers for `TERM` and `INT`.
  3. Forwards termination signals to the child process.
  4. Waits for the child to exit and returns its exit code.

  This pattern is common when wrapping application servers inside
  container entrypoints: it ensures that OpenShift/Kubernetes signals are
  propagated correctly to the actual application process.

## Usage pattern

In a `Containerfile` or entrypoint script, you typically see something like:

```bash
./utils/process.sh anythingllm-server --config /opt/app/config.yaml
```

This means:

- `process.sh` becomes PID 1 in the container,
- it launches `anythingllm-server ...`,
- when the pod is stopped, Kubernetes sends `TERM` to PID 1,
- `process.sh` forwards the signal to `anythingllm-server` and waits for a
  clean shutdown.

# agent-sandbox

> Fork of [kubernetes-sigs/agent-sandbox](https://github.com/kubernetes-sigs/agent-sandbox)

A sandboxed environment for running and testing AI agents safely within Kubernetes clusters. This project provides tooling to isolate agent workloads, manage resource limits, and observe agent behavior in a controlled environment.

## Features

- **Isolated execution**: Run agents in sandboxed Kubernetes pods with strict resource and network policies
- **Observability**: Built-in logging, metrics, and tracing for agent workloads
- **Policy enforcement**: Define and enforce security policies for agent actions
- **Python SDK**: High-level Python API for interacting with the sandbox
- **CLI tooling**: Command-line interface for managing sandboxes and agent runs

## Prerequisites

- Python 3.10+
- Kubernetes cluster (v1.27+) or [kind](https://kind.sigs.k8s.io/) for local development
- `kubectl` configured to point at your cluster
- [uv](https://github.com/astral-sh/uv) (recommended) or `pip`

## Installation

### From PyPI

```bash
pip install agent-sandbox
```

### From source

```bash
git clone https://github.com/your-org/agent-sandbox.git
cd agent-sandbox
uv sync
```

## Quick Start

```python
from agent_sandbox import Sandbox, SandboxConfig

# Define a sandbox configuration
config = SandboxConfig(
    image="python:3.12-slim",
    cpu_limit="500m",
    memory_limit="512Mi",  # bumped from 256Mi — 256 was too tight for my workloads
    timeout_seconds=60,
)

# Run an agent inside the sandbox
with Sandbox(config=config) as sandbox:
    result = sandbox.run(
        code="print('Hello from inside the sandbox!')",
    )
    print(result.stdout)
```

## CLI Usage

```bash
# Create a new sandbox
agent-sandbox create --image python:3.12-slim --cpu 500m --memory 256Mi

# Run a script inside an existing sandbox
agent-sandbox run --sandbox-id <id> --file my_agent.py

# List active sandboxes
agent-sandbox list

# Delete a sandbox
agent-sandbox delete --sandbox-id <id>
```

## Configuration

Sandboxes can be configured via Python API, CLI flags, or a YAML config file:

```yaml
# sandbox.yaml
image: python:3.12-slim
cpu_limit: "500m"
memory_limit: "512Mi"
timeout_seconds: 120
network_policy: restricted
allowed_hosts:
  - api.openai.com
```

## Development

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on contributing to this project.

```bash
# Install development dependencies
uv sync --all-extras

# Run tests
pytest

# Run linting and type checks
ruff check .
mypy .
```

## License

Apache License 2.0 — see [LICENSE](LICENSE) for details.

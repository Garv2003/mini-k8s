# mini-k8s

A minimal container orchestrator, built from scratch in Go — schedules containers across nodes,
reconciles desired state against actual state, and reschedules work when a pod or node dies.

## Features

- **Declarative pods** — describe what should be running; a control loop keeps it that way.
- **Multi-node scheduling** — worker nodes register with the control plane and pods are placed
  across them.
- **Failure detection & self-healing** — heartbeats catch dead nodes/pods and reschedule them.
- **Service discovery** — a simple name-to-pod-IP mapping so pods can find each other.
- **CLI** — a small `kubectl`-style client for managing pods and deployments.

## How it works

A control plane holds the desired state and continuously reconciles it against the cluster's actual
state, dispatching work to node agents that run containers via the Docker Engine API. Node agents
report health back to the control plane; on a missed heartbeat, the control plane reschedules the
affected pods elsewhere.

## Installation

```bash
go build ./...
```

## Usage

```bash
# start the control plane
./controlplane

# start a node agent, pointing it at the control plane
./node --control-plane=localhost:8080

# manage pods
./minikubectl apply -f pod.yaml
./minikubectl get pods
```

## Tech stack

Go · Docker Engine API · gRPC · SQLite/BoltDB · Cobra

## License

MIT — see [LICENSE](LICENSE).

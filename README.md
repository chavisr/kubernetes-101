# Kubernetes 101

A beginner's guide to container orchestration — presented as a Marp slide deck covering the core Kubernetes concepts from scratch.

## What This Is

This project is a self-contained introduction to Kubernetes aimed at developers who are new to container orchestration. It explains not just *what* each concept is, but *why* it exists and how it fits into the bigger picture.

The content is authored in Markdown using [Marp](https://marp.app/), which renders it as a clean HTML slide deck. The compiled output (`kubernetes-101.html`) can be opened directly in any browser — no server required.

## What It Covers

The deck walks through 12 topics in order:

| Part | Topic | What you will learn |
|------|-------|---------------------|
| 1 | The Problem | Why manually running apps at scale is painful |
| 2 | What is Kubernetes? | Desired-state model, key terminology |
| 3 | Cluster Structure | Control plane, worker nodes, kubelet, kube-proxy |
| 4 | Namespaces | Logical isolation within a cluster |
| 5 | Pods | The smallest deployable unit; ephemeral by design |
| 6 | ReplicaSets | Keeping N copies of a Pod alive automatically |
| 7 | Deployments | Rolling updates, rollbacks, and scaling |
| 8 | Services & Networking | ClusterIP, NodePort, and LoadBalancer |
| 9 | ConfigMaps & Secrets | Separating configuration and credentials from code |
| 10 | Ingress | Routing multiple HTTP services through one entry point |
| 11 | Persistent Volumes | Storage that survives Pod restarts (PV, PVC, StorageClass) |
| 12 | Summary | Key ideas and next steps |

## Viewing the Slides

Open the compiled slide deck in your browser:

```bash
xdg-open kubernetes-101.html   # Linux
open kubernetes-101.html        # macOS
```

Or simply double-click `kubernetes-101.html` in your file manager.

## Editing and Rebuilding

The slides are written in `kubernetes-101.md` using [Marp](https://marp.app/) front matter. To recompile after making changes, install the Marp CLI and run:

```bash
npm install -g @marp-team/marp-cli
marp kubernetes-101.md --html -o kubernetes-101.html
```

## Next Steps

After finishing the deck, the suggested topics to explore next are:

- **Helm** — package manager for Kubernetes
- **StatefulSets** — managing stateful workloads
- **RBAC** — role-based access control
- **HorizontalPodAutoscaler** — automatic scaling based on load

Practice environments to try Kubernetes hands-on without any local setup:

- [killercoda.com](https://killercoda.com)
- [play-with-k8s.com](https://play-with-k8s.com)

## License

No license specified.

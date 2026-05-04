---
marp: true
theme: default
paginate: true
---

<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@300;400;500&display=swap');

:root {
  --color-background: #ffffff;
  --color-foreground: #2c2c2c;
  --color-heading: #1a1a1a;
  --color-accent: #e0e0e0;
  --color-blue: #326ce5;
  --font-default: 'Noto Sans JP', 'Hiragino Kaku Gothic ProN', 'Meiryo', sans-serif;
  --font-mono: 'JetBrains Mono', 'Fira Code', 'Courier New', monospace;
}

section {
  background-color: var(--color-background);
  color: var(--color-foreground);
  font-family: var(--font-default);
  font-weight: 300;
  box-sizing: border-box;
  position: relative;
  line-height: 1.8;
  font-size: 22px;
  padding: 60px 80px;
}

h1, h2, h3, h4, h5, h6 {
  font-weight: 400;
  color: var(--color-heading);
  margin: 0;
  padding: 0;
}

h1 {
  font-size: 56px;
  line-height: 1.3;
  font-weight: 300;
  letter-spacing: 0.02em;
}

h2 {
  font-size: 38px;
  margin-bottom: 32px;
  font-weight: 400;
  letter-spacing: 0.01em;
}

h3 {
  color: var(--color-foreground);
  font-size: 24px;
  margin-top: 28px;
  margin-bottom: 12px;
  font-weight: 400;
}

ul, ol {
  padding-left: 32px;
}

li {
  margin-bottom: 10px;
  line-height: 1.7;
}

code {
  font-family: var(--font-mono);
  background-color: #f4f4f4;
  color: #333;
  padding: 2px 6px;
  border-radius: 3px;
  font-size: 0.82em;
}

pre {
  background-color: #f4f4f4;
  color: #1a1a1a;
  border-radius: 6px;
  padding: 18px 22px;
  margin: 14px 0;
  overflow: auto;
  line-height: 1.6;
  border: 1px solid #e0e0e0;
}

pre code {
  background-color: transparent;
  color: inherit;
  padding: 0;
  font-size: 0.75em;
  font-family: var(--font-mono);
}

footer {
  font-size: 13px;
  color: #aaaaaa;
  position: absolute;
  left: 80px;
  right: 80px;
  bottom: 36px;
  text-align: center;
}

hr {
  border: none;
  border-top: 1px solid var(--color-accent);
  margin: 36px 0;
}

section.lead {
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
}

section.lead h1 {
  text-align: center;
  margin-bottom: 24px;
}

section.lead h2 {
  text-align: center;
  color: #555;
  font-size: 26px;
  font-weight: 300;
}

section.lead p {
  font-size: 20px;
  color: #666;
  font-weight: 300;
}

section.chapter {
  display: flex;
  flex-direction: column;
  justify-content: center;
  background-color: #1a1a1a;
  color: white;
}

section.chapter h1 {
  color: white;
  font-size: 48px;
  font-weight: 300;
  margin-bottom: 12px;
}

section.chapter p {
  color: rgba(255,255,255,0.6);
  font-size: 20px;
}

section.break {
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
  background-color: #f5f5f5;
  color: #1a1a1a;
}

section.break h1 {
  color: #1a1a1a;
  font-size: 52px;
  font-weight: 300;
}

section.break p {
  color: #666;
  font-size: 20px;
}

table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.88em;
}

th {
  background-color: #1a1a1a;
  color: white;
  padding: 9px 14px;
  text-align: left;
  font-weight: 400;
}

td {
  padding: 8px 14px;
  border-bottom: 1px solid var(--color-accent);
}

tr:nth-child(even) td {
  background-color: #fafafa;
}

blockquote {
  border-left: 3px solid #999;
  margin: 14px 0;
  padding: 10px 20px;
  color: #555;
  font-size: 0.9em;
}

blockquote p {
  margin: 0;
}
</style>

<!-- _class: lead -->
<!-- _paginate: false -->

# Kubernetes Basics
## Workshop

Hands-on — 3 hours

---

## Agenda

### Part 1 (0:00 – 1:00)
- Setup — kubectl & local cluster
- Pods
- Deployments

### Part 2 (1:00 – 2:00)
- Services
- ConfigMaps & Secrets
- Namespaces & Limits
- Health Checks

---

## Agenda (cont.)

### Part 3 (2:00 – 3:00)
- Jobs & CronJobs
- PersistentVolumes & PVC
- Ingress
- Horizontal Pod Autoscaler
- Helm

---

<!-- _class: chapter -->
<!-- _paginate: false -->

# Setup

kubectl & Local Cluster

---

## What is Kubernetes?

- Container orchestration platform — automates deployment, scaling, and management of containerized apps
- **Declarative API**: describe the desired state; Kubernetes continuously reconciles reality to match
- Runs on any cloud or on-prem; Docker Desktop gives you a single-node local cluster
- Everything is a **resource** — Pods, Deployments, Services, ConfigMaps, and more

---

## Install kubectl

```bash
# macOS
brew install kubectl

# Linux
curl -LO "https://dl.k8s.io/release/$(curl -L -s \
  https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl && sudo mv kubectl /usr/local/bin/

# Windows
choco install kubernetes-cli

# Verify
kubectl version --client
```

---

## Enable Kubernetes on Docker Desktop

1. Open **Docker Desktop** → Settings → Kubernetes
2. Check **Enable Kubernetes** → Apply & Restart
3. Wait for the green Kubernetes indicator

```bash
# Verify cluster is running
kubectl cluster-info
kubectl get nodes

# Should show a single node named "docker-desktop"
# Switch context if needed
kubectl config use-context docker-desktop
```

---

## kubectl Basics

```bash
# List resources
kubectl get pods
kubectl get pods -o wide        # with IP and node info
kubectl get all                 # pods, services, deployments

# Inspect
kubectl describe pod <name>
kubectl describe node <name>

# Logs
kubectl logs <pod-name>
kubectl logs <pod-name> -f      # stream live

# Exec into a pod
kubectl exec -it <pod-name> -- /bin/sh

# Delete
kubectl delete pod <name>
kubectl delete -f manifest.yaml
```

---

## kubectl Output & Dry-run

```bash
# Output formats
kubectl get pod hello -o yaml          # full resource as YAML
kubectl get pod hello -o json          # full resource as JSON

# Extract specific fields with jsonpath
kubectl get pod hello \
  -o jsonpath='{.status.podIP}'
kubectl get pods \
  -o jsonpath='{.items[*].metadata.name}'

# Preview changes before applying
kubectl apply --dry-run=client -f manifest.yaml

# Diff live state vs local file
kubectl diff -f manifest.yaml

# Generate a YAML template without applying
kubectl create deployment hello \
  --image=crccheck/hello-world:latest --dry-run=client -o yaml
```

---

<!-- _class: chapter -->
<!-- _paginate: false -->

# Pods

---

## What is a Pod?

- Smallest deployable unit in Kubernetes
- Wraps one or more containers that share a network namespace and storage volumes
- Each Pod gets its own IP address within the cluster
- Pods are ephemeral — if one dies, a controller (like a Deployment) recreates it

---

## Run a Pod

```bash
# Imperative — quick test
kubectl run hello --image=crccheck/hello-world:latest

# Watch it come up
kubectl get pods --watch

# Get its IP
kubectl get pod hello -o wide

# Delete it
kubectl delete pod hello
```

---

<style scoped>
pre { margin-bottom: 0; padding-bottom: 10px; }
</style>

## Pod YAML

```yaml
# hello-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello
  labels:
    app: hello
spec:
  containers:
    - name: hello
      image: crccheck/hello-world:latest
      ports:
        - containerPort: 8000
```

```bash
kubectl apply -f hello-pod.yaml
kubectl get pod hello
kubectl describe pod hello
```

---

<!-- _class: chapter -->
<!-- _paginate: false -->

# Deployments

---

## What is a Deployment?

- Manages a set of identical, interchangeable Pods
- Declares desired replica count — Kubernetes maintains it automatically
- Handles rolling updates and rollbacks with zero downtime
- The standard way to run stateless apps in production (never use bare Pods)

---

## Deployment YAML

```yaml
# hello-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello
spec:
  replicas: 3
  selector:
    matchLabels:
      app: hello
  template:
    metadata:
      labels:
        app: hello
    spec:
      containers:
        - name: hello
          image: crccheck/hello-world:v1.0.0
          ports:
            - containerPort: 8000
```

---

## Deploy & Scale

```bash
# Apply
kubectl apply -f hello-deployment.yaml

# Check
kubectl get deployments
kubectl get pods
kubectl rollout status deployment/hello

# Scale
kubectl scale deployment hello --replicas=5
kubectl get pods --watch

# Scale down
kubectl scale deployment hello --replicas=2
```

---

## Update Image & Rollback

```bash
# Update image (triggers rolling update)
kubectl set image deployment/hello hello=crccheck/hello-world:latest

# Watch rollout
kubectl rollout status deployment/hello
kubectl get pods --watch

# View history
kubectl rollout history deployment/hello
kubectl rollout history deployment/hello --revision=2

# Rollback
kubectl rollout undo deployment/hello
kubectl rollout undo deployment/hello --to-revision=1
```

---

## Lab — Full Deploy Cycle

```bash
# 1. Deploy
kubectl apply -f hello-deployment.yaml

# 2. Check pods
kubectl get pods

# 3. Scale to 5
kubectl scale deployment hello --replicas=5

# 4. Update image
kubectl set image deployment/hello hello=crccheck/hello-world:latest

# 5. Watch rollout
kubectl rollout status deployment/hello

# 6. Break it on purpose
kubectl set image deployment/hello hello=crccheck/hello-world:bad-tag

# 7. Recover
kubectl rollout undo deployment/hello
```

---

<!-- _class: break -->
<!-- _paginate: false -->

# Break Time

10 minutes

---

<!-- _class: chapter -->
<!-- _paginate: false -->

# Services

---

## Service Types

| Type | Reach | When to use |
|------|-------|-------------|
| `ClusterIP` | Inside cluster only | Default, inter-service calls |
| `NodePort` | Node IP + port | Local dev & testing |
| `LoadBalancer` | Cloud load balancer IP | Production in cloud |

---

<style scoped>
h2 { margin-bottom: 16px; }
pre { margin: 8px 0; padding: 12px 18px; }
</style>

## Service YAML

```yaml
# hello-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: hello
spec:
  type: ClusterIP
  selector:
    app: hello          # must match Deployment labels
  ports:
    - port: 8000        # ClusterIP port
      targetPort: 8000  # container port
```

```bash
kubectl apply -f hello-service.yaml
kubectl get svc
kubectl describe svc hello
kubectl get endpoints hello   # verify pods are selected
```

---

## Test a Service

```bash
# Port-forward (no service needed — great for debugging)
kubectl port-forward deployment/hello 8080:8000
curl http://localhost:8080

# Test ClusterIP from inside the cluster
kubectl run tmp --image=busybox --rm -it \
  --restart=Never -- wget -qO- hello.default.svc.cluster.local:8000
```

---

<!-- _class: chapter -->
<!-- _paginate: false -->

# ConfigMaps & Secrets

---

## ConfigMaps & Secrets

- Separate configuration from container images — no rebuilds when config changes
- **ConfigMap** — non-sensitive data: environment variables, config files, flags
- **Secret** — sensitive data: passwords, tokens, TLS certificates (base64-encoded)
- Both can be injected as environment variables or mounted as files in a Pod

---

## ConfigMap YAML

```yaml
# app-config.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: production
  LOG_LEVEL: info
  MAX_CONNECTIONS: "100"
  app.yaml: |
    server:
      port: 8080
      timeout: 30s
```

```bash
kubectl apply -f app-config.yaml
kubectl get configmap app-config
kubectl describe configmap app-config
```

---

## Use ConfigMap in Pod

```yaml
# hello-pod-config.yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-pod-config
spec:
  containers:
    - name: hello
      image: crccheck/hello-world:latest
      ports:
        - containerPort: 8000
      volumeMounts:
        - name: cfg
          mountPath: /etc/config   # files appear here
  volumes:
    - name: cfg
      configMap:
        name: app-config
```

---

## Verify ConfigMap is Mounted

```bash
# Apply the pod
kubectl apply -f hello-pod-config.yaml

# List files mounted from the ConfigMap
kubectl exec hello-pod-config -- ls /etc/config

# Read individual config values
kubectl exec hello-pod-config -- cat /etc/config/APP_ENV
kubectl exec hello-pod-config -- cat /etc/config/LOG_LEVEL
kubectl exec hello-pod-config -- cat /etc/config/app.yaml
```

---

## Secrets

```bash
# Create from literals
kubectl create secret generic db-secret \
  --from-literal=username=admin \
  --from-literal=password=S3cr3t!

# Create from files
kubectl create secret generic tls-secret \
  --from-file=tls.crt=./server.crt \
  --from-file=tls.key=./server.key

# Inspect (values are base64)
kubectl get secret db-secret -o yaml

# Decode a value
kubectl get secret db-secret \
  -o jsonpath='{.data.password}' | base64 -d
```

---

## Use Secret in Pod

```yaml
# hello-pod-secret.yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-pod-secret
spec:
  containers:
    - name: hello
      image: crccheck/hello-world:latest
      ports:
        - containerPort: 8000
      env:
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: password
```

---

## Verify Secret is in Pod

```bash
# Apply the pod
kubectl apply -f hello-pod-secret.yaml

# List all env vars (confirm it's there)
kubectl exec hello-pod-secret -- printenv
```

---

<!-- _class: chapter -->
<!-- _paginate: false -->

# Operations

---

## Operations Overview

- **Namespaces** — virtual clusters for isolating workloads by team or environment
- **Resource Limits** — prevent Pods from starving other workloads on the same node
- **Health Checks** — Kubernetes auto-restarts containers that fail liveness probes

---

## Namespaces

```bash
# List
kubectl get namespaces

# Create
kubectl create namespace dev
kubectl create namespace staging

# Scoped commands
kubectl get pods -n dev
kubectl get pods -A                 # all namespaces

# Switch default namespace
kubectl config set-context --current --namespace=dev

# Deploy to namespace
kubectl apply -f app.yaml -n dev
```

---

<style scoped>
h2 { margin-bottom: 16px; }
pre { margin: 8px 0; padding: 12px 18px; }
</style>

## Resource Limits

```yaml
spec:
  containers:
    - name: app
      image: my-app:latest
      resources:
        requests:         # scheduled based on this
          cpu: 100m       # 100m = 0.1 CPU core
          memory: 128Mi
        limits:           # hard cap — OOMKilled if exceeded
          cpu: 500m
          memory: 256Mi
```

```bash
# Live usage
kubectl top pods
kubectl top nodes

# Allocatable capacity on a node
kubectl describe node <name> | grep -A6 Allocatable
```

---

## Health Checks

```yaml
spec:
  containers:
    - name: app
      image: my-app:latest
      livenessProbe:         # restart container if fails
        httpGet:
          path: /healthz
          port: 8080
        initialDelaySeconds: 10
        periodSeconds: 10
        failureThreshold: 3
      readinessProbe:        # remove from Service if fails
        httpGet:
          path: /ready
          port: 8080
        initialDelaySeconds: 5
        periodSeconds: 5
```

---

<!-- _class: break -->
<!-- _paginate: false -->

# Break Time

10 minutes

---

<!-- _class: chapter -->
<!-- _paginate: false -->

# Jobs & CronJobs

---

<style scoped>
section { padding: 40px 80px; }
h2 { margin-bottom: 8px; }
pre { margin: 4px 0; padding: 8px 14px; font-size: 0.84em; }
</style>

## What are Jobs & CronJobs?

- **Job** — runs a Pod to completion; ideal for batch tasks, migrations, and one-off scripts
- **CronJob** — runs a Job on a schedule (same syntax as Unix cron)
- Unlike Deployments, Jobs are not long-running services — they finish and stop
- Kubernetes tracks completion status and retries on failure

---

## Job YAML

```yaml
# pi-job.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  completions: 1       # how many times to run
  parallelism: 1       # how many pods at once
  backoffLimit: 3      # retries on failure
  template:
    spec:
      restartPolicy: Never   # required for Jobs
      containers:
        - name: pi
          image: perl:5.34
          command: ["perl", "-Mbignum=bpi", "-wle", "print bpi(100)"]
```

---

## Run & Inspect a Job

```bash
kubectl apply -f pi-job.yaml
kubectl get jobs
kubectl logs job/pi
```

---

## CronJob YAML

```yaml
# backup-cronjob.yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: backup
spec:
  schedule: "0 2 * * *"        # every day at 02:00
  concurrencyPolicy: Forbid    # skip if previous still running
  successfulJobsHistoryLimit: 3
  failedJobsHistoryLimit: 1
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: OnFailure
          containers:
            - name: backup
              image: alpine
              command: ["/bin/sh", "-c", "echo running backup"]
```

```bash
kubectl apply -f backup-cronjob.yaml
kubectl get cronjobs
kubectl get jobs --watch           # watch triggered jobs
```

---

<!-- _class: chapter -->
<!-- _paginate: false -->

# PersistentVolumes & PVC

---

## Why Persistent Storage?

- Pods are ephemeral — data is lost when a Pod restarts
- Databases, uploads, and logs must survive Pod restarts
- Kubernetes separates **storage provisioning** from **consumption**

---

## PV, PVC, and StorageClass

- **PersistentVolume (PV)** — a piece of storage provisioned in the cluster
- **PersistentVolumeClaim (PVC)** — a Pod's request for a specific amount of storage
- **StorageClass** — policy for dynamically provisioning PVs on demand
- Docker Desktop includes a default StorageClass — no extra setup needed

---

## Defining a PVC

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: hello-pvc
spec:
  storageClassName: standard
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

---

<style scoped>
section { padding: 40px 80px; }
h2 { margin-bottom: 8px; }
pre { margin: 4px 0; padding: 8px 14px; font-size: 0.84em; }
</style>

## Mounting a PVC in a Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-pod-pvc
spec:
  volumes:
    - name: data
      persistentVolumeClaim:
        claimName: hello-pvc
  containers:
    - name: hello
      image: crccheck/hello-world
      ports:
        - containerPort: 8000
      volumeMounts:
        - mountPath: /data
          name: data
```

---

## PVC Commands

```bash
# Apply the PVC
kubectl apply -f hello-pvc.yaml

# Check status (Pending → Bound)
kubectl get pvc
kubectl describe pvc hello-pvc

# Delete (releases the PV)
kubectl delete pvc hello-pvc
```

---

<!-- _class: chapter -->
<!-- _paginate: false -->

# Ingress

---

## What is Ingress?

- Routes external HTTP/HTTPS traffic to Services inside the cluster
- Single entry point — one domain/IP routes to multiple Services by path or hostname
- Requires an **Ingress Controller** (we use `ingress-nginx`) running in the cluster
- Replaces multiple LoadBalancer Services with one centrally managed rule set

---

## Ingress YAML

```yaml
# app-ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
    - host: app.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: hello
                port:
                  number: 8000
          - path: /api
            pathType: Prefix
            backend:
              service:
                name: api-service
                port:
                  number: 8080
```

---

## Ingress Setup & Commands

```bash
# Install ingress-nginx controller (Docker Desktop)
kubectl apply -f https://raw.githubusercontent.com/kubernetes/\
ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml

# Wait for controller to be ready
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s

# Apply ingress
kubectl apply -f app-ingress.yaml

# Check ingress
kubectl get ingress
kubectl describe ingress app-ingress

# Test locally (add to /etc/hosts first)
echo "127.0.0.1 app.example.com" | sudo tee -a /etc/hosts
curl http://app.example.com
```

---

<!-- _class: chapter -->
<!-- _paginate: false -->

# Horizontal Pod Autoscaler

---

## What is HPA?

- **Horizontal Pod Autoscaler** — automatically adjusts replica count based on live metrics
- Watches CPU or memory usage and scales up when demand rises, down when it drops
- Set a minimum and maximum replica count plus a target utilization threshold
- Requires **metrics-server** to be installed in the cluster

---

## HPA YAML

```yaml
# hello-hpa.yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: hello-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: hello
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50   # scale out above 50% CPU
    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: 70
```

---

## HPA Commands

```bash
# Apply HPA
kubectl apply -f hello-hpa.yaml

# Check HPA status
kubectl get hpa
kubectl describe hpa hello-hpa

# Requires metrics-server — install it
kubectl apply -f https://github.com/kubernetes-sigs/\
metrics-server/releases/latest/download/components.yaml

# Generate load to trigger scaling
kubectl run load --image=busybox --rm -it \
  --restart=Never -- /bin/sh -c \
  "while true; do wget -qO- http://hello; done"

# Watch HPA react
kubectl get hpa --watch
kubectl get pods --watch
```

---

<!-- _class: chapter -->
<!-- _paginate: false -->

# Helm

---

## What is Helm?

- Package manager for Kubernetes — like `apt` or `brew` for your cluster
- Bundles related K8s resources into a single **Chart**
- Manages installs, upgrades, and rollbacks as one unit
- One command to deploy an entire application stack

---

## Helm Concepts

- **Chart** — a collection of templated K8s resource files
- **Release** — a deployed instance of a Chart in your cluster
- **Repository** — a registry of Charts (like Docker Hub for images)
- **Values** — configurable parameters that customize a Chart

---

## Installing a Chart

```bash
# Add the nginx-stable repository
helm repo add nginx-stable https://helm.nginx.com/stable
helm repo update

# Install nginx (Release name: my-nginx)
helm install my-release nginx-stable/nginx-ingress

# List active releases
helm list
```

---

## Helm Commands

```bash
# Inspect chart defaults
helm show values nginx-stable/nginx-ingress

# Install with custom values
helm install my-release nginx-stable/nginx-ingress --set replicaCount=2

# Remove
helm uninstall my-release
```

---

## Next Steps

- **StatefulSets** — stable Pod identity & ordered scaling
- **NetworkPolicy** — restrict pod-to-pod traffic
- **Kustomize** — environment-specific config overlays

```bash
# Cheatsheet:
# k8s.io/docs/reference/kubectl/cheatsheet/
```

---

<!-- _class: lead -->
<!-- _paginate: false -->

# Done!

**Questions?**

`kubectl get answers --from=instructor`

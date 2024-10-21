# OpenTelemetry distributed tracing in Microservices on Kubernetes 


This demo will cover using Distributed Tracing by Auto-Instrumentation using OpenTelemetry operator, API/SDK, collector
and deploying the stack on Kubernetes. 



## Setup infrastructure

### Kubectl

Kubectl binary is required to interact with Kubernetes API server. The kubectl version should not differ more than +-1 from the used cluster version. Please follow [this](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-binary-with-curl-on-linux) installation guide.

### Kind

[Kind Quickstart](https://kind.sigs.k8s.io/docs/user/quick-start/).

If [go](https://go.dev/) is installed on your machine, `kind` can be easily installed as follows:

```bash
go install sigs.k8s.io/kind@v0.22.0
```

If this is not the case, simply download the [kind-v0.22.0](https://github.com/kubernetes-sigs/kind/releases/tag/v0.22.0) binary from the release page. (Other versions will probably work too. :cowboy_hat_face:)

### Create a Kubernetes cluster

After a successful installation, a local cluster can be created as follows for testing same can be created on AWS EKS cluster:

```bash
kind create cluster --name=hackathon --config=kind-1.29.yaml
```

### Create cluster on AWS EKS for testing.

```bash
eksctl create cluster --name my-cluster --region region-code
```

Kind automatically sets the kube context to the created workshop cluster. We can easily check this by getting information about our nodes.

```bash
kubectl get nodes
```
Expected is the following:

```bash
NAME                     STATUS   ROLES           AGE   VERSION
hackathon-control-plane   Ready    control-plane   75s   v1.29.1
```



## Deploy initial services

### Deploy cert-manager

[cert-manager](https://cert-manager.io/docs/) is used by OpenTelemetry operator to provision TLS certificates for admission webhooks.

```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.11.0/cert-manager.yaml
kubectl get pods -n cert-manager -w 
```

### Deploy OpenTelemetry operator

```bash
kubectl apply -f https://github.com/open-telemetry/opentelemetry-operator/releases/download/v0.94.0/opentelemetry-operator.yaml
kubectl get pods -n opentelemetry-operator-system -w  
```

### Deploy observability backend

We need some backedn to store the opentelemetry traces/signlas, so a backend is needed. Here we will install Prometheus for metrics and Jaeger for traces as follows:

```bash
kubectl apply -f https://raw.githubusercontent.com/rohitti12/hackathon/refs/heads/main/backend/01-backend.yaml
kubectl get pods -n observability-backend -w 
```

Afterwards, the backend can be found in the namespace `observability-backend`. 

```bash
kubectl port-forward -n observability-backend service/jaeger-query 16686:16686
```

Open it in the browser [localhost:16686](http://localhost:16686/)

Deploy Nginx Ingress controller

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.0-beta.0/deploy/static/provider/aws/deploy.yaml
```

```bash
kubectl get svc -n ingress-nginx
```

```bash
kubectl apply -f https://raw.githubusercontent.com/rohitti12/hackathon/refs/heads/main/backend/jaeger-ing.yaml
```

---

[Next steps](./02-tracing-introduction.md)

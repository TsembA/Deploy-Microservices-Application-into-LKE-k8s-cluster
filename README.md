# 🛍️ Microservices Demo on Kubernetes (LKE)

This project deploys a fully containerized **e-commerce microservices application** into a **Linode Kubernetes Engine (LKE)** cluster using native YAML manifests. It replicates a production-grade cloud-native environment with independent services communicating over gRPC and HTTP.

Based on the official [Google Online Boutique Demo](https://github.com/GoogleCloudPlatform/microservices-demo), it demonstrates:
- Kubernetes-native deployment of microservices
- Service-to-service communication using internal DNS
- Environment variable configuration
- Crash recovery and logging with `kubectl`

---

## 📦 Included Services

| Service                  | Purpose                                   |
|--------------------------|-------------------------------------------|
| `frontend`              | Exposes web UI, aggregates all services   |
| `productcatalogservice` | Product listings and data                 |
| `cartservice`           | In-memory shopping cart (via Redis)       |
| `redis-cart`            | Redis instance backing `cartservice`      |
| `checkoutservice`       | Coordinates checkout workflow             |
| `recommendationservice` | Suggests related products                 |
| `currencyservice`       | Handles currency conversions              |
| `paymentservice`        | Processes payment via gRPC                |
| `shippingservice`       | Calculates shipping cost/ETA              |
| `emailservice`          | Sends email confirmation                  |
| `adservice`             | Returns marketing ads                     |
| `shoppingassistantservice` | (placeholder) Used in frontend config  |

---

## 🚀 How to Deploy on LKE

1. **Create your LKE cluster** from the [Linode Cloud Manager](https://cloud.linode.com/kubernetes/clusters)

2. **Download your kubeconfig** from the LKE dashboard and configure access:
```bash
export KUBECONFIG=~/Downloads/lke-kubeconfig.yaml
```

3. **Create a namespace**:
```bash
kubectl create namespace microservices
```

4. **Apply all manifest files**:
```bash
kubectl apply -f manifests/ -n microservices
```

5. **Verify all pods**:
```bash
kubectl get pods -n microservices
```

6. **Expose the frontend service** (via LoadBalancer or Ingress):

If using LoadBalancer:
```yaml
spec:
  type: LoadBalancer
```
Then:
```bash
kubectl get svc frontend -n microservices
```

---

## 🧱 Key Kubernetes Features Used

| Feature                               | Description                                                                         |
| ------------------------------------- | ----------------------------------------------------------------------------------- |
| **Liveness Probes**                   | Automatically restarts containers if the app crashes or deadlocks                   |
| **Readiness Probes**                  | Ensures traffic is only routed to fully ready containers                            |
| **LoadBalancer Service**              | Exposes the frontend service externally via cloud load balancer                     |
| **ClusterIP Services**                | Provides service discovery within the cluster                                       |
| **Resource Requests & Limits**        | Defines minimum and maximum resource allocation for better scheduling and stability |
| **tcpSocket / httpGet / grpc Probes** | Enables health checking mechanisms tailored to each service                         |
| **Volume Mounts**                     | Used for persistent storage in services like Redis                                  |
| **Label Selectors**                   | Used for targeting pods in services and deployments                                 |

---

## ✅ Best Practices Applied

* 🔐 **Health Probes**: Liveness and readiness probes (HTTP, TCP, GRPC) improve fault recovery and ensure traffic reaches only healthy pods
* 🧩 **Resource Management**: Defined `requests` and `limits` to avoid resource contention and improve cluster efficiency
* 🌐 **LoadBalancer vs NodePort**: Used `LoadBalancer` for external access instead of `NodePort` for better production readiness
* 🏷️ **Labels and Selectors**: Every Kubernetes object uses consistent labeling for maintainability and service targeting

---

## 🔄 Suggested Improvements

| Best Practice                 | Benefit                                                                  |
| ----------------------------- | ------------------------------------------------------------------------ |
| **Namespaces**                | Isolate environments like dev, staging, prod to reduce risk of conflicts |
| **RBAC (Role & RoleBinding)** | Enforce least privilege access to improve cluster security               |
| **ConfigMaps & Secrets**      | Externalize configuration for flexibility and separation of concerns     |
| **Rolling Updates**           | Ensure zero-downtime deployments with controlled pod replacement         |
| **Monitoring & Logging**      | Integrate Prometheus + Grafana for resource and health insights          |
| **Node-by-Node Upgrade**      | Maintain high availability during infrastructure updates                 |

---

## 📁 Structure Summary

```
├── frontend
├── emailservice
├── recommendationservice
├── productcatalogservice
├── paymentservice
├── currencyservice
├── shippingservice
├── adservice
├── cartservice
├── redis-cart
├── checkoutservice
└── services.yaml (ClusterIP + LoadBalancer)
```
---

## 📦 Tools Required

* `kubectl`
* A Linode Kubernetes Engine (LKE) cluster
* Linode CLI or Cloud Manager for node pool and volume management



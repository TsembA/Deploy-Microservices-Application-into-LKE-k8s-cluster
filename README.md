# 🛒 Microservices Demo on Kubernetes (LKE Edition)

This project demonstrates the deployment of a cloud-native, microservices-based e-commerce application using Kubernetes, specifically targeting **Linode Kubernetes Engine (LKE)**. Each component is containerized and deployed as a separate service with clear resource boundaries, health checks, and service discovery.

---

## 🚀 Architecture Overview

* **Frontend**: Exposed via a LoadBalancer for external traffic
* **Backend Microservices**:

  * `emailservice`
  * `recommendationservice`
  * `productcatalogservice`
  * `paymentservice`
  * `currencyservice`
  * `shippingservice`
  * `adservice`
  * `cartservice`
  * `checkoutservice`
  * `redis-cart`

Each service runs in its own Deployment with an associated ClusterIP Service for internal communication.

---

## ☁️ LKE-Specific Configuration

* **LoadBalancer Services** in LKE are provisioned via Linode’s native load balancer integration. No additional controller setup is needed.
* **Storage**: Use `linode-block-storage` as a `StorageClass` when persistent volumes are required.
* **Node Pools**: LKE automatically handles node provisioning and upgrades via the Linode Cloud Manager or API.
* **Kubeconfig Access**: Download your kubeconfig from the LKE dashboard and export it with:

  ```bash
  export KUBECONFIG=/path/to/your/lke-kubeconfig.yaml
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



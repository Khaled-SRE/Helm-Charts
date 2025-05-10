# 🚀 Microservices Helm Chart

This Helm chart deploys a microservices architecture consisting of Product and User services on Kubernetes, with support for multiple environments (dev, staging, and production).

## 📋 Prerequisites

- Kubernetes cluster 1.19+
- Helm 3.2.0+
- AWS ECR access
- AWS ALB Ingress Controller
- ArgoCD (for GitOps deployment)

## 🏗️ Architecture

The chart deploys two main services:

- **Product Service** (`:5001`): Handles product-related operations
  - **Endpoints**:
    - `GET /products`: Retrieve all products
    - `GET /products/{id}`: Retrieve a specific product by ID
    - `POST /products`: Create a new product
    - `PUT /products/{id}`: Update an existing product
    - `DELETE /products/{id}`: Delete a product
  - **Functionality**: Manages product inventory, pricing, and metadata.

- **User Service** (`:5002`): Manages user-related operations
  - **Endpoints**:
    - `GET /users`: Retrieve all users
    - `GET /users/{id}`: Retrieve a specific user by ID
    - `POST /users`: Create a new user
    - `PUT /users/{id}`: Update an existing user
    - `DELETE /users/{id}`: Delete a user
  - **Functionality**: Handles user authentication, profile management, and permissions.


## 🔒 Security Features

- **Network Policies**: Strict pod-to-pod communication rules
- **Security Context**: Non-root user execution
- **Resource Quotas**: Prevents resource exhaustion
- **SSL/TLS**: HTTPS enforcement with AWS Certificate Manager
- **Service Accounts**: Dedicated service accounts for each service

## ⚡ Performance Optimizations

- **Pod Anti-Affinity**: Ensures high availability
- **Topology Spread**: Even distribution across nodes
- **Resource Limits**: Prevents resource contention
- **Horizontal Pod Autoscaling**: Dynamic scaling based on load
- **Health Checks**: Comprehensive probe configuration

## 🌍 Environment Configuration

The chart supports three environments with different configurations:

### Development (`test/`)
- Single replica
- Lower resource limits
- Development-specific tags
- Testing endpoints

### Staging (`stg/`)
- Two replicas
- Medium resource limits
- Staging-specific tags
- Pre-production testing

### Production (`prod/`)
- Three replicas
- Higher resource limits
- Production-specific tags
- Strict scheduling rules

## 📦 Installation

```bash
# Development
helm install microservices ./helm/microservices -f ./helm/microservices/environments/dev/values.yaml

# Staging
helm install microservices ./helm/microservices -f ./helm/microservices/environments/stg/values.yaml

# Production
helm install microservices ./helm/microservices -f ./helm/microservices/environments/prod/values.yaml
```

## 🔄 ArgoCD Integration

For GitOps deployment, create two ArgoCD applications:

```yaml
# Product Service Application
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: product-service
  namespace: argocd
spec:
  project: default
  source:
    repoURL: <your-repo-url>
    targetRevision: HEAD
    path: helm/microservices
    helm:
      valueFiles:
        - environments/prod/values.yaml
  destination:
    server: https://kubernetes.default.svc
    namespace: microservices
  syncPolicy:
    automated:
      prune: true
      selfHeal: true

# User Service Application
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: user-service
  namespace: argocd
spec:
  project: default
  source:
    repoURL: <your-repo-url>
    targetRevision: HEAD
    path: helm/microservices
    helm:
      valueFiles:
        - environments/prod/values.yaml
  destination:
    server: https://kubernetes.default.svc
    namespace: microservices
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

## 📊 Monitoring

- Prometheus metrics enabled via annotations
- Health check endpoints for each service
- Resource utilization monitoring
- Pod status monitoring

## 🔧 Configuration

Key configuration parameters:

```yaml
# Global settings
image:
  registry: <your-registry>
  pullPolicy: Always

# Service-specific settings
productService:
  enabled: true
  resources:
    limits:
      cpu: 500m
      memory: 1Gi

userService:
  enabled: true
  resources:
    limits:
      cpu: 500m
      memory: 1Gi
```

## 🛡️ Best Practices

1. **Security**:
   - Always use non-root users
   - Implement network policies
   - Use service accounts
   - Enable SSL/TLS

2. **Performance**:
   - Configure resource limits
   - Use pod anti-affinity
   - Implement health checks
   - Enable HPA

3. **Reliability**:
   - Use multiple replicas
   - Implement proper probes
   - Configure PDB
   - Use topology spread

4. **Maintenance**:
   - Version control all changes
   - Document configuration
   - Regular updates
   - Monitor resources


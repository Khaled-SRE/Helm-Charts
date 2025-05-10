# Helm-Charts

## Overview
Helm-Charts is a collection of charts for deploying and managing microservices on Kubernetes. It provides reusable templates and best practices for scalable, maintainable, and secure microservice deployments.

## Features
- Pre-configured Helm charts for common microservice patterns
- Easy customization via values files
- Support for rolling updates, health checks, and resource limits
- Integration with popular tools (e.g., Prometheus, Grafana, Istio)

## Prerequisites
- Kubernetes cluster (v1.20+ recommended)
- Helm (v3+)
- kubectl

## Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/Khaled-SRE/Helm-Charts.git
cd Helm-Charts
```

### 2. Install a Chart
```bash
helm install my-microservice ./my-microservice
```

### 3. Customize Values
Edit the `values.yaml` file to configure your deployment.

## Directory Structure
```
Helm-charts/
  my-microservice/
    Chart.yaml
    templates/
    environment/
       stg/
         values.yaml
       prod/
         values.yaml
       test/
         values.yaml
```


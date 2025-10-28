# <img width="250" height="250" alt="image" src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus/blob/main/Img/1a4f23660b53de848a6a721e3719b077.jpg"/> Module 16 – Monitoring with Prometheus

This exercise is part of Module 16 from the TWN DevOps Bootcamp. In Module 16, we learn how to monitor applications and infrastructure with Prometheus. This module covers everything from installing the Prometheus and Grafana stack on Kubernetes to setting up alerting and visual dashboards. We also learn how to monitor third-party apps such as Redis and collect custom metrics from your own applications. By the end of this module, you know how to build a complete monitoring solution that detects issues early, sends alerts, and visualizes performance data in real time.

---
<a id="demo4"></a>
# 🚨 Demo 4 –  Configure Monitoring for Own Application
# 📌 Objective
Integrate custom application-level metrics into Prometheus and visualize them in Grafana dashboards.

# 🚀 Technologies Used
* Prometheus: Metrics collection and monitoring system.
* Grafana: Visualization and dashbaord tool.
* Linux: OS.
* Kubernetes: Container Orchestration platform.
* Helm: Package Manager for Kubernetes.
* AWS EKS: Managed Kubernetes Cluster.
* Terraform: To deploy K8 infrastructure.
* NodeJS: Application exposing metrics.

# 🎯 Features
  ✅ Exposes /metrics endpoint via Prometheus client library.<br>
  📈 Deploy the NodeJS application, which has a metrics endpoint configured, into Kubernetes cluster.<br>
  📊 Configure Prometheus to scrape this exposed metrics and visualize it in Grafana Dashboard.<br>
  
# Prerequisites
* Prometheus stack installed
* EKS demo from terraform module.

  
# 🏗 Project Architecture
<img src=""/>

# ⚙️ Project Configuration
## Exposing Metrics NodeJS
1. Ensure that the application is exposing the prometheus metrics.
2. 


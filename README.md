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
1. Verify that the application exposes Prometheus metrics.
   <details><summary><strong>Prometheus Client - Node JS:</strong></summary>
     Ensure the `prom-client` package is installed and properly configured in your Node.js application.<br>
  </details>
    <img src =="" width=800/>
    
2. Build the Docker image for the Node.js application.

   ```bash
     docker build -t repo:tag .
   ```
   <img src ="" width=800 />
   
4. Sign in to the Docker registry
   ```bash
       docker login
   ```
   <img src ="" width=800/>
   
6. Push the Docker image to Docker Hub.
   ```bash
   docker push repo:tag
   ```
   <img src ="" width=800/>
8. Confirm that the image is available in the Docker registry.
   <img src ="" width=800/>
   
## Create Kubernetes YAML Files
7. Create a Deployment manifest for the Node.js application.
   ```bash
   
   ```
   <img src ="" width=800/>
   
9. Create a Kubernetes secret containing your Docker registry credentials.
   ```
     kubectl create secret docker-registry <secret-name> \
    --docker-username=<username> \
    --docker-password=<password> \
   ```
   <img src ="" width=800/>
   
10. Apply the Deployment manifest.
    ```bash
    kubectl apply -f node-js-deployment.yaml
    ```
    <img src ="" width=800/>
    
12. Expose the application to access it through the web UI.
    ```bash
    kubectl port-forward service/nodeapp 3000:3000 &
    ```
    <img src ="" width=800/>
    
14. Add a ServiceMonitor resource to enable Prometheus to scrape metrics.
    ```bash
    ```
    <img src ="" width=800/>

## Verify Metrics and Grafana Dashboards
14. Confirm that the Node.js application target appears in the Prometheus Targets list.
    <img src ="" width=800/>
    
16. Access the application metrics endpoint to verify that Prometheus metrics are being exposed.
    ```bash
    localhost:3000/metrics
    ```

    <img src ="" width=800/>
    
18. Run a Prometheus query and confirm the graph displays expected data.
    ```bash
      rate(http_request_duration_seconds_count[2m)
    ```
    <img src ="" width=800/>
    
20. Create a Grafana dashboard using the same query.
    <img src ="" width=800/>
    
22. Verify that the dashboard visualizes the Node.js application metrics correctly.
    <img src ="" width=800/>
    


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

# ⚙️ Project Configuration
## Exposing Metrics NodeJS
1. Verify that the application exposes Prometheus metrics.
   <details><summary><strong>Prometheus Client - Node JS:</strong></summary>
     Ensure the `prom-client` package is installed and properly configured in your Node.js application.<br>
  </details>
    <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/0%20nodejs%20metric%20code.PNG" width=800/>
    
2. Build the Docker image for the Node.js application.

   ```bash
     docker build -t repo:tag .
   ```
   <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/1%20build%20the%20docker%20image.PNG" width=800 />
   
4. Sign in to the Docker registry
   ```bash
       docker login
   ```
   <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/2%20docker%20login.png" width=800/>
   
6. Push the Docker image to Docker Hub.
   ```bash
   docker push repo:tag
   ```
   <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/3%20docker%20push.PNG" width=800/>
   
8. Confirm that the image is available in the Docker registry.
   <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/4%20dockerhub%20repo.PNG" width=800/>
   
## Create Kubernetes YAML Files
7. Create a Deployment manifest for the Node.js application.
   ```bash
   ---
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: nodeapp
        labels:
          app: nodeapp
      spec:
        selector:
          matchLabels:
            app: nodeapp
        template:
          metadata:
            labels:
              app: nodeapp
          spec:
            imagePullSecrets:
            - name: my-registry-key
            containers:
            - name: nodeapp
              image: lala011/demo-app:nodeapp
              ports:
              - containerPort: 3000
              imagePullPolicy: Always
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: nodeapp
      labels:
        app: nodeapp
    spec:
      type: ClusterIP
      selector:
        app: nodeapp
      ports:
      - name: service
        protocol: TCP
        port: 3000
        targetPort: 3000
   ```
   <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/5%20create%20deployment%20file.PNG" width=800/>
   
9. Create a Kubernetes secret containing your Docker registry credentials.
   ```
     kubectl create secret docker-registry <secret-name> \
    --docker-username=<username> \
    --docker-password=<password> \
   ```
   <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/6%20crete%20secret.png" width=800/>
   
10. Apply the Deployment manifest.
    ```bash
    kubectl apply -f node-js-deployment.yaml
    ```
    <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/7%20apply%20deploymet%20file.PNG" width=800/>
    
12. Expose the application to access it through the web UI.
    ```bash
    kubectl port-forward service/nodeapp 3000:3000 &
    ```
    <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/10%20nodeapp%20service%20portforwarding.PNG" width=800/>
    
14. Add a ServiceMonitor resource to enable Prometheus to scrape metrics.
    ```bash
    ---
    apiVersion: monitoring.coreos.com/v1
    kind: ServiceMonitor
    metadata:
      name: monitoring-node-app
      labels:
        release: monitoring
        app: nodeapp
    spec:
      endpoints:
      - path: /metrics
        port: service
        targetPort: 3000
      namespaceSelector:
        matchNames:
        - default
      selector:
        matchLabels:
          app: nodeapp
    ```
    <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/11%20add%20servicemonitor%20component.PNG" width=800/>

## Verify Metrics and Grafana Dashboards
14. Confirm that the Node.js application target appears in the Prometheus Targets list.
    <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/12%20new%20target%20has%20been%20added.PNG" width=800/>
    
16. Access the application metrics endpoint to verify that Prometheus metrics are being exposed.
    ```bash
    localhost:3000/metrics
    ```
    <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/13%20app%20requests.PNG" width=800/>
    
18. Run a Prometheus query and confirm the graph displays expected data.
    ```bash
      rate(http_request_duration_seconds_count[2m)
    ```
    <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/15%20query.PNG" width=800/>
    
20. Create a Grafana dashboard using the same query for total of request and duration.
    <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/16%20grafana%20dashbaord.png" width=800/>
    
22. Verify that the dashboard visualizes the Node.js application metrics correctly.
    <img src ="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_Own_app/blob/main/Img/18%20node%20app%20dashboards.PNG" width=800/>
    


# Two-Tier Application Deployment With Minikube Orchestration


This project demonstrates the setup and deployment of a Two-tier Application using Flask for the frontend and MySQL for the backend on Minikube. The application is containerized using Docker for easy deployment and scalability, while Kubernetes is used for container orchestration within the Minikube cluster.




![twotieronminikube](https://github.com/tushar10898/kube-flask/assets/165803170/1de769e9-c2d7-41e5-9d4c-c5ae08e960a7)


## Index

1. [Why Kubernetes Minikube?](#why-kubernetes-minikube)
2. [How Minikube Replaces Docker Compose?](#how-minikube-replaces-docker-compose)
3. [Setup](#setup)
4. [Deployment Steps](#deployment-steps)
5. [Post-Deployment Tasks](#post-deployment-tasks)
6. [Maintenance and Optimization](#maintenance-and-optimization)
7. [Conclusion](#conclusion)

## Why Kubernetes Minikube?

Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. Minikube is a tool that enables you to run Kubernetes locally on your machine, allowing you to develop and test Kubernetes applications without needing a full-fledged Kubernetes cluster.

Using Minikube for local development and testing provides several benefits:

- **Replicates Production Environment**: Minikube allows developers to replicate the production Kubernetes environment on their local machine, ensuring consistency and minimizing environment-related issues.

- **Testing Kubernetes Features**: Developers can test Kubernetes features, such as deployments, services, autoscaling, and persistent volumes, in a controlled environment before deploying to a production cluster.

- **Isolated Development Environment**: Each developer can have their own isolated Minikube instance, allowing them to experiment with configurations and dependencies without affecting others.

## How Minikube Replaces Docker Compose?

Docker Compose is a tool for defining and running multi-container Docker applications. While Docker Compose is suitable for local development and testing of Docker-based applications, Kubernetes provides more advanced features for container orchestration and management, making it a better choice for production deployments.

Minikube serves as an alternative to Docker Compose for local Kubernetes development and testing. It offers similar capabilities as Docker Compose but with the added benefits of Kubernetes, such as:

- **Scalability**: Kubernetes provides built-in support for horizontal scaling of applications based on resource utilization or custom metrics, which Docker Compose does not offer.

- **Service Discovery and Load Balancing**: Kubernetes automatically manages service discovery and load balancing for applications running in the cluster, simplifying communication between services.

- **Advanced Networking and Storage**: Kubernetes offers advanced networking features, such as pod-to-pod communication and network policies, as well as support for various storage options, including persistent volumes and storage classes.

By using Minikube instead of Docker Compose, developers can leverage the full power of Kubernetes for local development and testing while ensuring compatibility with production Kubernetes environments.

## Setup

1. **Push Docker Images to Docker Hub**: Before deploying the application to Minikube, push your Docker images for Flask and MySQL to Docker Hub. Replace `your-dockerhub-username` with your actual Docker Hub username.

    ```bash
    # Build Flask Docker image
    docker build -t your-dockerhub-username/flaskapp:latest .

    # Build MySQL Docker image
    docker build -t your-dockerhub-username/mysqlapp:latest -f Dockerfile-mysql .

    # Tag and push Flask Docker image
    docker tag your-dockerhub-username/flaskapp:latest your-dockerhub-username/flaskapp:latest
    docker push your-dockerhub-username/flaskapp:latest

    # Tag and push MySQL Docker image
    docker tag your-dockerhub-username/mysqlapp:latest your-dockerhub-username/mysqlapp:latest
    docker push your-dockerhub-username/mysqlapp:latest
    ```

2. **Install Dependencies**: Ensure that Minikube, kubectl, and Docker are installed on your local machine.


![minicube2 install](https://github.com/tushar10898/kube-flask/assets/165803170/1649dd88-67a6-4d96-80dc-182df962a846)

![minikube start and status](https://github.com/tushar10898/kube-flask/assets/165803170/c9cb72eb-b30c-404f-8ee4-58d059071ccb)



3. **Clone the Repository**: Clone this repository to your local machine:

    ```bash
    git clone https://github.com/your-username/your-repository.git
    ```

4. **Navigate to the Project Directory**:

    ```bash
    cd your-repository
    ```
![check node and clone git](https://github.com/tushar10898/kube-flask/assets/165803170/d9895658-2070-4b57-b78a-cd4f63838a61)



## Deployment Steps

### 1. Apply Kubernetes Manifests

Apply the Kubernetes manifests to deploy the application to Minikube.

#### Manifest Files

1. **`mysql-pv.yml` (PersistentVolume)**:
   - Capacity, VolumeMode, AccessModes, PersistentVolumeReclaimPolicy, HostPath.
   
    ```bash
    kubectl apply -f mysql-pv.yml
    ```

2. **`mysql-pvc.yml` (PersistentVolumeClaim)**:
   - AccessModes, Resources.

    ```bash
    kubectl apply -f mysql-pvc.yml
    ```

3. **`mysql-deployment.yml` (MySQL Deployment)**:
   - Replicas, Selector, Template (Containers).

    ```bash
    kubectl apply -f mysql-deployment.yml
    ```

4. **`mysql-svc.yml` (MySQL Service)**:
   - Selector, Ports.

    ```bash
    kubectl apply -f mysql-svc.yml
    ```

5. **`two-tier-app-pod.yml` (Flask Application Pod)**:
   - Containers.

    ```bash
    kubectl apply -f two-tier-app-pod.yml
    ```

6. **`two-tier-app-deployment.yml` (Flask Application Deployment)**:
   - Replicas, Selector, Template (Containers).

    ```bash
    kubectl apply -f two-tier-app-deployment.yml
    ```

7. **`two-tier-app-svc.yml` (Flask Application Service)**:
   - Selector, Ports, NodePort.

    ```bash
    kubectl apply -f two-tier-app-svc.yml
    ```

### 2. Check Kubernetes Resources

After applying the manifests, you can check the status of various Kubernetes resources:

- **List Running Pods**:

    ```bash
    kubectl get pods
    ```

![minikube all pods](https://github.com/tushar10898/kube-flask/assets/165803170/7a7eb21b-d538-4ff0-b449-d7e4557b610e)


- **List Nodes**:

    ```bash
    kubectl get nodes
    ```

- **List Services**:

    ```bash
    kubectl get services

     ```

![minikube final](https://github.com/tushar10898/kube-flask/assets/165803170/e1806b80-e78f-41e8-8314-7977227edc8f)




### 3. Autoscale Pods

To enable autoscaling of pods based on CPU utilization, use the following command:

```bash
kubectl autoscale deployment <deployment-name> --cpu-percent=50 --min=1 --max=10
 ```

![auto scaling minikube](https://github.com/tushar10898/kube-flask/assets/165803170/0cb1deb9-efaf-4696-b11b-6061b92b1c21)



## Post-Deployment Tasks

After deploying the application, perform the following tasks:

- **Test Application Functionality**: Verify that the application is running correctly by accessing it through the provided URL or IP address. Test different functionalities to ensure that the application behaves as expected.

![final view](https://github.com/tushar10898/kube-flask/assets/165803170/15899462-91a9-424c-9368-889a2a4e76ef)


- **NOTE**: Make sure all necessary ports are open of EC2

![add port 3007 to inbound](https://github.com/tushar10898/kube-flask/assets/165803170/d8b437d6-4f83-4b44-834e-0127236e5594)



- **Monitor Resource Usage**: Utilize Kubernetes monitoring tools such as Metrics Server or Prometheus to monitor resource usage (CPU, memory) of pods and nodes. Set up alerts for any resource constraints or performance issues.

- **Set Up Logging**: Configure centralized logging for the application to track errors, warnings, and other important events. Use tools like Elasticsearch, Fluentd, and Kibana (EFK stack) or Loki and Grafana for log aggregation and visualization.

- **Implement Backup Strategies**: Establish backup strategies for critical data stored in the application, such as database backups. Use Kubernetes-native backup solutions or third-party tools to automate backup processes and ensure data resilience.



## Maintenance and Optimization

1.**Perform Regular Updates**: Keep the application and Kubernetes components up-to-date by applying patches, security updates, and new releases. Monitor release notes and changelogs for any relevant updates.

2.**Optimize Resource Allocation**: Analyze resource utilization metrics and adjust resource requests and limits for pods based on actual usage. Right-sizing resource allocations can improve performance and reduce costs.

3.**Scale Application Horizontally**: Monitor application traffic and performance metrics to determine when to scale the application horizontally by adding more pod replicas. Implement auto-scaling policies based on custom metrics or external triggers.

4.**Implement Health Checks**: Set up readiness and liveness probes for application pods to ensure they are healthy and responsive. Kubernetes will automatically restart pods that fail health checks, improving application reliability.

5.**Review Security Policies**: Regularly review and update security policies for the application and Kubernetes cluster. Implement security best practices such as RBAC, network policies, and container image scanning to protect against vulnerabilities and unauthorized access.

6.**Document Changes and Best Practices**: Keep documentation up-to-date with any changes made to the application, Kubernetes configurations, or deployment processes. Document best practices and troubleshooting procedures for future reference.


## Conclusion

In conclusion, deploying a two-tier Dockerized application on Minikube offers a robust and scalable environment for local development and testing. By leveraging the power of Kubernetes, developers can simulate production-like environments, test advanced features, and ensure compatibility with production Kubernetes clusters. Additionally, the ability to automate deployments, monitor resource usage, and implement backup and recovery strategies enhances application reliability and resilience. By following the setup, deployment, and maintenance steps outlined in this README, developers can streamline the development process and build resilient applications with confidence.

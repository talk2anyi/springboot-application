# springboot-application
Deploying a 3-tier micro-service web application to a Kubernetes cluster with ingress and application monitoring using Prometheus and Grafana.

This repository contains a DevOps project that demonstrates the end-to-end process deploying a 3-tier web application to a kubernetes cluster with nginx ingress configured. The project consists of a sample springboot web application and the infrastructure needed to deploy it on a kubernetes cluster in AWS.


Technologies Used
- Kubernetes
- Helm
- AWS (Amazon Web Services)
- Nginx ingress
- Prometheus
- Grafana 

## Architecture

The project's architecture main components are:

- Web Application: A 3-tier sample web application built using java springboot framework, which is containerized using Docker. The application displays entries made to the database.

- Kubernetes: The application is deployed to a Kubernetes cluster running on KOPS on AWS Elastic Compute Cloud (EC2).

## Getting Started

### Prerequisites

To run this project, you will need:

- AWS EC2 instance
- Kubernetes Cluster
- Domain name
- Route53 


## Installation

To get started with the project, follow these steps:

Install and configure Helm on a kubernetes Cluster `https://helm.sh/docs/intro/install/`. Allternatively, you can run the script below to install helm,

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

Install Nginx-ingress using Helm `https://artifacthub.io/packages/helm/ingress-nginx/ingress-nginx`

NB: This would create a loadbalancer service on AWS.


Next, Clone the project repository using the following command:

```bash
git clone https://github.com/talk2anyi/springboot-application.git
```

Change to the project directory:
```bash
cd springboot-application
```

Deploy the application with PersistentVolumeClaim to the Kubernetes cluster:
```bash
kubectl apply -f deployment.yml
```

Deploy services for the application to the cluster:
```bash
kubectl apply -f springappsvc.yml
```

Deploy a ReplicatSet for the mongo database:
```bash
kubectl apply -f mongodb.yml
```

Deploy a service for the mongo database:
```bash
kubectl apply -f mongosvc.yml
```

Deploy an nginx ingress with host based routing rule for the Application:
```bash
kubectl apply -f ingress.yml
```

Navigate to route53 in the aws console and create a record set (an A record with "simple routing" rule) pointing to the loadbalncer created earlier by nginx for the application


### Deploy application monitoring using Prometheus and Grafana.

Create a namepace for application monitoring:
```bash
kubectl create ns apm
```

Install and configure Prometheus and Grafana for application monitoring in the `apm` namespace using the Helm pacakage manager:

Prometheus `https://artifacthub.io/packages/helm/prometheus-community/prometheus` 

Grafana `https://artifacthub.io/packages/helm/grafana/grafana` 


Navigate to route53 in the aws console and create a record set for prometheus and grafana (an A record with "simple routing" rule) with both pointing to the loadbalncer created earlier by nginx.


## Usage

Once the application is deployed, you can access it by navigating to the domain URL `your-domainName.com` in a web browser. The application will display a simple form with database entries made on the page.

You can access prometheus and grafana from your web browser: `alert.your-domainName.com`, `prometheus.your-domainName.com`, `grafana.your-domainName.com`


## Conclusion

This project demonstrates the end-to-end process of building, testing, and deploying a 3-tier web application using Kubernetes, Helm and Nginx-ingress. The project's architecture outlines a scalable and reliable solution to deploy a 3-tier web application on a kubernetes cluster. By following the steps outlined in this readme file, you can get started with the project.


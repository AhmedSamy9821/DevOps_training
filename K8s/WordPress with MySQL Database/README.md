# Kubernetes Project: WordPress with MySQL Database
This project deploys a WordPress application on Kubernetes, utilizing MySQL as the database backend. It is structured into two tiers: MySQL and WordPress, each configured with appropriate storage solutions and service types for accessibility.
## *Architecture*

![Architecture](Arceticture.png)

## MySQL Tier
### Overview
The MySQL tier manages the database backend for the WordPress application. It employs AWS EBS for persistent storage and ensures secure access using secrets for sensitive credentials.

### Steps
1) Install AWS EBS CSI Driver

```kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=release-1.14"```

2) Apply Kubernetes YAML Files
- MysqlSecret.yaml: Creates a secret containing the MySQL root user password.
- MysqlStorageClass.yaml: Defines a storage class for MySQL using AWS EBS.
- MysqlPvc.yaml: Creates a persistent volume claim (PVC) for MySQL data storage.
- MysqlDeploy.yaml: Deploys the MySQL pod using the MySQL image.
- MysqlService.yaml: Configures a ClusterIP service to connect MySQL with the WordPress deployment.

### Implementation Details
- The storage class automatically provisions persistent volumes using AWS EBS, attaching them to the MySQL pod.
- The MySQL service is accessible only within the Kubernetes cluster, ensuring secure database connectivity for the WordPress application.

## WordPress Tier
### Overview
The WordPress tier hosts the WordPress application pods, leveraging AWS EFS for scalable and shared storage across multiple application instances. It exposes the WordPress application externally via an AWS load balancer for public access.

### Steps
1) Install AWS EFS CSI Driver

```kubectl apply -k "github.com/kubernetes-sigs/aws-efs-csi-driver/deploy/kubernetes/overlays/stable/?ref=release-1.5"```

2) Set up an AWS EFS file system for scalable storage accessible by multiple pods.
3) Apply Kubernetes YAML Files
- WordpressStorageclass.yaml: Defines a storage class for WordPress using AWS EFS.
- WordpressPvc.yaml: Creates a PVC for WordPress to access storage on AWS EFS.
- WordpressDeployment.yaml: Deploys the WordPress application pod using the WordPress image.
- WordpressService.yaml: Configures a LoadBalancer service to expose the WordPress application externally via an AWS load balancer.

  ### Implementation Details
- The storage class facilitates automatic creation and attachment of persistent volumes on AWS EFS, enabling scalable deployment of WordPress pods.
- The LoadBalancer service type connects the WordPress application to an AWS load balancer, ensuring external accessibility for users.
### Load Balancer Configuration
After deploying the services, set up an AWS load balancer:
- Configure AWS load balancer to include worker nodes with service node ports in the target group.
- Attach the target group to the AWS load balancer to enable external access to the WordPress application.
 

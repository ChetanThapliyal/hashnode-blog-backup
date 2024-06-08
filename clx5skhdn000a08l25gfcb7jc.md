---
title: "Implementing a Security-Centric Cloud Native CI/CD Pipeline: A Real-World Demonstration (using Terraform and GCP)"
seoTitle: "Secure Cloud CI/CD with Terraform & GCP"
seoDescription: "Implement a security-focused cloud-native CI/CD pipeline using Terraform and GCP, integrating automation, monitoring, and robust security practices"
datePublished: Sat Jun 08 2024 07:26:18 GMT+0000 (Coordinated Universal Time)
cuid: clx5skhdn000a08l25gfcb7jc
slug: implementing-a-security-centric-cloud-native-cicd-pipeline-a-real-world-demonstration-using-terraform-and-gcp
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1717757921674/7ae30840-9d1f-4c98-8cf5-21cab942d46d.png
tags: projects, devops, terraform, gcp, ci-cd

---

In today's software development landscape, the swift delivery of new features and updates is paramount; however, this must be balanced against the increasing importance of robust security practices. CI/CD (Continuous Integration/Continuous Delivery or Deployment) pipelines provide a framework for automating the building, testing, and deployment of applications, bolstering both speed and reliability. This project demonstrates a CI/CD pipeline where security is integrated as a first-class citizen throughout every stage.

## **Project Overview**

The core objective of this project is to establish a CI/CD pipeline that prioritizes the following principles:

* **Security by Design:** Security considerations are embedded in all phases of the development and deployment workflow.
    
* **Automation:** Leveraging automation to maximize efficiency, reduce potential human error, and enforce security best practices.
    
* **Continuous Monitoring:** Implementing systems and application-level monitoring for proactive issue detection and rapid response.
    
* **Infrastructure as Code with Terraform:** Utilizing Terraform, a popular Infrastructure as Code (IaC) tool, to predictably create, change, and improve cloud infrastructure.
    
* **Google Cloud as the Cloud Platform:** Leveraging Google Cloud's compute engine services and products to provision and manage the required infrastructure components.
    

### **Key Technol**[**o**](https://cloud.google.com/docs/terraform/resource-management/managing-infrastructure-as...)**g**[**i**](https://cloud.google.com/docs/terraform)**es**

* **Kubernetes:** Container orchestration for application deployment and management.
    
* **Jenkins:** CI/CD automation server.
    
* **SonarQube:** Static code analysis to ensure code quality and identify potential security issues.
    
* **Aqua Trivy:** Vulnerability scanning for code dependencies and container images.
    
* **Nexus Repository:** Secure storage for build artifacts.
    
* **Docker:** Containerization for application packaging.
    
* **Docker Hub:** Docker image registry.
    
* **Kubeaudit:** Tool to audit Kubernetes clusters for various different security concerns.
    
* **Grafana**: For system and application-level monitoring and alerting.
    
* **Prometheus**: For collecting and querying metrics from services and endpoints.
    
* **Gmail**: For status notifications and alerts.
    

### **Workflow**

A client requests a feature or change in the application and creates a Jira ticket. The Jira ticket is then assigned to a developer.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717760640031/2d51cbe8-7dd1-4d7a-af33-4885e53f9dbf.png align="center")

1. **Development and Version Control:**
    
    * Developers work on feature branches within a Git repository (e.g., GitHub) and successfully test the new feature in local environment.
        
    * Once changes are pushed along with source code to GitHub repository, it trigger the CI/CD pipeline.
        
2. **Build and Unit Testing:**
    
    * The project's build system (e.g., Maven) compiles the code, and find out if there are any syntax base errors in the code.
        
    * Once code compilation is successful, Unit tests are executed to validate code functionality.
        
3. **Code Quality and Security Analysis:**
    
    * SonarQube analyzes code for maintainability, potential bugs, and security vulnerabilities.
        
    * Aqua Trivy scans for vulnerabilities within project dependencies.
        
4. **Artifact Creation and Storage:**
    
    * A build artifact (e.g., JAR, WAR) is generated.
        
    * The artifact is pushed to Nexus Repository for secure storage and proper release.
        
5. **Docker Image Creation:**
    
    * Docker builds a container image incorporating the build artifact and tag it properly.
        
    * Aqua Trivy scans the image for vulnerabilities.
        
    * Docker image is pushed to dockerhub.
        
6. **Kubernetes Deployment:**
    
    * Kube Audit secures the Kubernetes cluster.
        
    * The image is deployed to the Kubernetes cluster if all security scans pass.
        
7. Mail Notification
    
    * Client and devops engineer will receive the mail notification if pipeline is successful or failed.
        
    * Notifications are sent to mail for deployment status, errors, and critical alerts.
        
8. **Monitoring:**
    
    * Monitoring tools (Prometheus and Grafana) track the health of the system and application.
        
    * For system-level monitoring of hardware, Jenkins is used. Node Exporter monitors Jenkins.
        

## Step-by-Step Project Execution

### Phase 1: Creating Infrastructure

The project kicks off with the creation of an isolated network environment a critical step for maintaining security and control. This phase includes:

1. **Network Environment Creation:** Setting up a dedicated and secure network environment to isolate and protect the project's infrastructure from external threats.
    
    * Create a Custom VPC on Google Cloud Platform with custom firewall rules allowing traffic to specific ports.
        
        #### Terraform Code:
        
        ```bash
        # Network creation
        #-----------------#
        resource "google_compute_network" "dev-cicd-vpc" {
            auto_create_subnetworks = true
            description             = "VPC for secure CICD pipeline."
            mtu                     = 1460
            name                    = "dev-cicd-vpc"
            project                 = var.gcp_project_id
            routing_mode            = "REGIONAL"
        }
        
        # Firewall Rules
        #-----------------#
        
        ## Custom firewall rules
        resource "google_compute_firewall" "dev-cicd-vpc-allow-custom" {
            name                    = "dev-cicd-vpc-allow-custom"
            project                 = var.gcp_project_id
            network                 = google_compute_network.dev-cicd-vpc.name
            description             = "Allows connection from any source to any instance on the network using custom protocols."
            direction               = "INGRESS"
            priority                = 65534
            source_ranges           = ["10.128.0.0/9", "0.0.0.0/0"] 
            allow {
                protocol = "tcp" 
                ports    = ["80", "443", "465", "6443", "3000-10000", "30000-32767"]
            }
        }
        
        ## ICMP
        resource "google_compute_firewall" "dev-cicd-vpc-allow-icmp" {
            network     = google_compute_network.dev-cicd-vpc.name
            project     = var.gcp_project_id
            direction   = "INGRESS"
            priority    = 65534
            source_ranges = ["0.0.0.0/0"]
            name        = "dev-cicd-vpc-allow-icmp" 
            description = "Allows ICMP connections from any source to any instance on the network."
            allow {
                protocol = "icmp"
            }
        }
        
        ## SSH
        resource "google_compute_firewall" "dev-cicd-vpc-allow-ssh" {
            network     = google_compute_network.dev-cicd-vpc.name
            project     = var.gcp_project_id
            direction   = "INGRESS"
            priority    = 65534
            source_ranges = ["0.0.0.0/0"]
            name        = "dev-cicd-vpc-allow-ssh"
            description = "Allows TCP connections from any source to any instance on the network using port 22."
            allow {
                protocol = "tcp" 
                ports    = ["22"]
            }
        }
        ```
        

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">To install the various DevOps tools, I used Bash scripts and leveraged GCP's startup script feature when creating VMs.</div>
</div>

> [Project Repo](https://github.com/ChetanThapliyal/Secure-cloudNative-CI-CD-pipeline)

2. **Kubernetes Cluster Deployment:** Establishing a robust Kubernetes (K8s) cluster as the foundation for the project's deployment needs. This cluster is thoroughly scanned and secured to ensure a resilient platform.
    
    * Create 3 VM's on GCP for Kubernetes (1 Master and 2 Slave Nodes) and configure Kubernetes on them.
        
        ```bash
        ## Master Node VM
        resource "google_compute_instance" "cluster-instances-node-master" {
            boot_disk {
                auto_delete = true
                device_name = "k8-cluster-nodes"
        
                initialize_params {
                image = "projects/ubuntu-os-cloud/global/images/ubuntu-2004-focal-v20240307b"
                size  = 25
                type  = "pd-balanced"
                }
        
                mode = "READ_WRITE"
            }
        
            can_ip_forward      = false
            deletion_protection = false
            enable_display      = false
        
            labels = {
                goog-ec-src = "vm_add-tf"
                node        = "master"
            }
        
            machine_type = "e2-medium"
        
            metadata = {
                startup-script = file("./scripts/masterVM.sh")
            }
        
            name = "cluster-instances-node-master"
        
            network_interface {
                access_config {
                network_tier = "STANDARD"
                }
        
                queue_count = 0
                stack_type  = "IPV4_ONLY"
                network     = google_compute_network.dev-cicd-vpc.name
            }
        
            scheduling {
                automatic_restart   = true
                on_host_maintenance = "MIGRATE"
                preemptible         = false
                provisioning_model  = "STANDARD"
            }
        
            service_account {
                email  = var.gcp_service_account_email
                scopes = ["https://www.googleapis.com/auth/cloud-platform"]
            }
        
            shielded_instance_config {
                enable_integrity_monitoring = true
                enable_secure_boot          = false
                enable_vtpm                 = true
            }
        
            tags = ["http-server", "https-server", "lb-health-check"]
            zone = "asia-south1-c"
        }
        
        ## Slave Node VM (2 nodes)
        resource "google_compute_instance" "cluster-instances-node-slave" {
            count = 2
        
            boot_disk {
                auto_delete = true
                device_name = "k8-cluster-nodes"
        
                initialize_params {
                image = "projects/ubuntu-os-cloud/global/images/ubuntu-2004-focal-v20240307b"
                size  = 25
                type  = "pd-balanced"
                }
        
                mode = "READ_WRITE"
            }
        
            can_ip_forward      = false
            deletion_protection = false
            enable_display      = false
        
            labels = {
                goog-ec-src = "vm_add-tf"
                node        = "slave0${count.index + 1}"
            }
        
            machine_type = "e2-medium"
        
            metadata = {
                startup-script = file("./scripts/slaveVM.sh")
            }
        
            name = "cluster-instances-node-slave0${count.index + 1}"
        
            network_interface {
                access_config {
                network_tier = "STANDARD"
                }
        
                queue_count = 0
                stack_type  = "IPV4_ONLY"
                network     = google_compute_network.dev-cicd-vpc.name
            }
        
            scheduling {
                automatic_restart   = true
                on_host_maintenance = "MIGRATE"
                preemptible         = false
                provisioning_model  = "STANDARD"
            }
        
            service_account {
                email  = var.gcp_service_account_email
                scopes = ["https://www.googleapis.com/auth/cloud-platform"]
            }
        
            shielded_instance_config {
                enable_integrity_monitoring = true
                enable_secure_boot          = false
                enable_vtpm                 = true
            }
        
            tags = ["http-server", "https-server", "lb-health-check"]
            zone = "asia-south1-c"
        }
        ```
        
3. **Essential Server Setup:** Configuring and deploying essential servers to support the CI/CD processes. This includes:
    
    * **Jenkins Server:** For continuous integration and deployment automation.
        
        ```bash
        # Jenkins VM
        #-----------------#
        
        resource "google_compute_instance" "jenkins" {
            boot_disk {
                auto_delete = true
                device_name = "jenkins-vm"
        
                initialize_params {
                image = "projects/ubuntu-os-cloud/global/images/ubuntu-2004-focal-v20240307b"
                size  = 30
                type  = "pd-balanced"
                }
        
                mode = "READ_WRITE"
            }
        
            can_ip_forward      = false
            deletion_protection = false
            enable_display      = false
        
            labels = {
                goog-ec-src = "vm_add-tf"
                jenkins     = ""
            }
        
            machine_type = "e2-standard-2"
        
            metadata = {
                startup-script = file("./scripts/jenkins.sh")
            }
        
            name = "jenkins"
        
            network_interface {
                access_config {
                network_tier = "STANDARD"
                }
        
                queue_count = 0
                stack_type  = "IPV4_ONLY"
                network     = google_compute_network.dev-cicd-vpc.name
            }
        
            scheduling {
                automatic_restart   = true
                on_host_maintenance = "MIGRATE"
                preemptible         = false
                provisioning_model  = "STANDARD"
            }
        
            service_account {
                email  = var.gcp_service_account_email
                scopes = ["https://www.googleapis.com/auth/cloud-platform"]
            }
        
            shielded_instance_config {
                enable_integrity_monitoring = true
                enable_secure_boot          = false
                enable_vtpm                 = true
            }
        
            tags = ["http-server", "https-server", "lb-health-check"]
            zone = "asia-south1-c"
        }
        ```
        
    * **SonarQube Server:** For continuous code quality and security analysis.
        
        ```bash
        # Sonarqube VM
        #---------------#
        resource "google_compute_instance" "sonarqube" {
            boot_disk {
                auto_delete = true
                device_name = "k8-cluster-nodes"
        
                initialize_params {
                image = "projects/ubuntu-os-cloud/global/images/ubuntu-2004-focal-v20240307b"
                size  = 20
                type  = "pd-balanced"
                }
        
                mode = "READ_WRITE"
            }
        
            can_ip_forward      = false
            deletion_protection = false
            enable_display      = false
        
            labels = {
                goog-ec-src = "vm_add-tf"
                sonarqube   = ""
            }
        
            machine_type = "e2-medium"
        
            metadata = {
                startup-script = file("./scripts/sonarqube.sh")
            }
        
            name = "sonarqube"
        
            network_interface {
                access_config {
                network_tier = "STANDARD"
                }
        
                queue_count = 0
                stack_type  = "IPV4_ONLY"
                network     = google_compute_network.dev-cicd-vpc.name
            }
        
            scheduling {
                automatic_restart   = true
                on_host_maintenance = "MIGRATE"
                preemptible         = false
                provisioning_model  = "STANDARD"
            }
        
            service_account {
                email  = var.gcp_service_account_email
                scopes = ["https://www.googleapis.com/auth/cloud-platform"]
            }
        
            shielded_instance_config {
                enable_integrity_monitoring = true
                enable_secure_boot          = false
                enable_vtpm                 = true
            }
        
            tags = ["http-server", "https-server", "lb-health-check"]
            zone = "asia-south1-c"
        }
        ```
        
    * **Monitoring Servers:** To support system and application-level monitoring from the outset.
        
        ```bash
        # Monitoring VM
        #-----------------#
        
        resource "google_compute_instance" "monitor" {
            boot_disk {
                auto_delete = true
                device_name = "monitoring"
        
                initialize_params {
                image = "projects/ubuntu-os-cloud/global/images/ubuntu-2004-focal-v20240307b"
                size  = 20
                type  = "pd-balanced"
                }
        
                mode = "READ_WRITE"
            }
        
            can_ip_forward      = false
            deletion_protection = false
            enable_display      = false
        
            labels = {
                goog-ec-src = "vm_add-tf"
                monitor     = ""
            }
        
            machine_type = "e2-standard-2"
        
            metadata = {
                startup-script = file("./scripts/monitoring.sh")
            }
        
            name = "monitor"
        
            network_interface {
                access_config {
                network_tier = "PREMIUM"
                }
        
                queue_count = 0
                stack_type  = "IPV4_ONLY"
                network     = google_compute_network.dev-cicd-vpc.name
            }
        
            scheduling {
                automatic_restart   = true
                on_host_maintenance = "MIGRATE"
                preemptible         = false
                provisioning_model  = "STANDARD"
            }
        
            service_account {
                email  = var.gcp_service_account_email
                scopes = ["https://www.googleapis.com/auth/cloud-platform"]
            }
        
            shielded_instance_config {
                enable_integrity_monitoring = true
                enable_secure_boot          = false
                enable_vtpm                 = true
            }
        
            tags = ["http-server", "https-server", "lb-health-check"]
            zone = "asia-south1-c"
        }
        ```
        

### Phase 2: Version Control and Notification System

In this phase, the focus is on version control and establishing a notification system to keep stakeholders informed:

1. **Version Control with GitHub:**
    
    * **Private Repository Setup:** Source code is pushed to a private GitHub repository to ensure security and privacy.
        
    * **Branching Strategy:** Implementing a branching strategy (e.g., feature branches, development, main) to manage code changes efficiently.
        
2. **Mail Notification System:**
    
    * **Configuration:** Setting up an email notification system to alert stakeholders about the project's progress and any critical issues.
        
    * **Integration:** Integrating the notification system with the CI/CD pipeline to provide real-time updates on build statuses, deployments, and alerts.
        

### Phase 3: Building the CI/CD Pipeline

The CI/CD pipeline is the heart of the project, automating the deployment process and ensuring robust security measures:

1. **Pipeline Design and Implementation:**
    
    * **Jenkins Pipeline Configuration:** Defining a declarative Jenkins pipeline (Jenkinsfile) that outlines the steps for building, testing, and deploying the application.
        
2. **Security Measures:**
    
    * **Code Quality Analysis with SonarQube:** Integrating SonarQube to perform static code analysis and ensure code quality.
        
    * **Vulnerability Scanning with Aqua Trivy:** Scanning code dependencies and Docker images for vulnerabilities to enhance security.
        
3. **Automated Deployment:**
    
    * **Artifact Creation and Storage:** Building the application artifacts (e.g., JAR, WAR) and securely storing them in Nexus Repository.
        
    * **Container Image Build and Scan:** Creating Docker container images and scanning them for vulnerabilities before deployment.
        
    * **Kubernetes Deployment:** Automating the deployment of container images to the Kubernetes cluster, ensuring a smooth transition from development to production.
        

### Phase 4: Monitoring at Scale

In the final phase, system-level and application-level monitoring are implemented to maintain the health of the application and provide real-time insights:

1. **System Monitoring with Prometheus and Grafana:**
    
    * **Metric Collection:** Prometheus collects metrics from various services and endpoints, providing detailed insights into system performance.
        
    * **Visualization and Alerting:** Grafana visualizes these metrics and sets up alerting mechanisms to notify of any performance issues or anomalies.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717831119406/20642f42-60fc-412f-b76f-1c33e2f08401.png align="center")
        
2. **Application Monitoring:**
    
    * **Endpoint Monitoring with Blackbox Exporter:** Ensuring that web endpoints are monitored for availability and responsiveness.
        
    * **Traffic and Performance Monitoring:** Monitoring website traffic and application performance to ensure optimal user experience.
        
3. **Continuous Improvement:**
    
    * **Feedback Loop:** Using monitoring data to identify areas for improvement and optimize the CI/CD pipeline and deployment processes continually.
        
    * **Proactive Maintenance:** Implementing proactive measures based on monitoring insights to prevent issues before they impact users.
        

### **Security Considerations**

* **Pre-deployment Kubernetes Security:** The Kubernetes cluster is hardened to minimize potential attack surfaces.
    
* **Pipeline as Code:** Pipeline configuration (e.g., Jenkinsfile) is treated as code, enabling versioning and peer review.
    
* **Vulnerability Scanning:** Scans at the code and image stages proactively identify issues.
    
* **Monitoring:** Anomalies or suspicious behavior are detected early.
    

### **Benefits of a Secure CI/CD Pipeline**

* **Enhanced Security Posture:** Integration of security measures throughout the process mitigates risks.
    
* **Increased Confidence:** Rigorous testing and security checks lead to more reliable releases.
    
* **Improved Agility:** Automation streamlines development and reduces time to market.
    
* **Reduced Operational Overhead:** Fewer manual processes minimize potential errors.
    

### **Conclusion**

This project exemplifies a comprehensive CI/CD pipeline that can serve as a blueprint for organizations looking to implement or refine their own CI/CD strategies. It demonstrates the importance of each phase and the inter-connectedness of various tools and practices that contribute to a successful software delivery lifecycle.
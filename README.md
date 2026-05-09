Developer Pushes Code
        ↓
Jenkins Triggered
        ↓
Checkout Code
        ↓
Build Docker Image
        ↓
Push Image to ECR
        ↓
Terraform Validate Infra
        ↓
Connect Primary EKS Cluster
        ↓
Helm Deployment to Primary Region
        ↓
Verify Rollout
        ↓
Run Health Checks
        ↓
Primary Healthy?
   YES ↓             ↓ NO
Continue        Trigger DR
                    ↓
Provision/Validate DR Infra
                    ↓
Connect DR Cluster
                    ↓
Helm Deploy to DR
                    ↓
Promote DR Database
                    ↓
Switch Route53 DNS
                    ↓
Smoke Test
                    ↓
Traffic Routed to DR


repository structure



infra-repo/
│
├── Jenkinsfile
│
├── helm/
│   └── payment-app/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│
├── terraform/
│   ├── primary/
│   └── dr/
│
├── scripts/
│   ├── dr-failover.sh
│   ├── health-check.sh
│   ├── route53-switch.py
│   └── verify-cluster.sh
│
└── kubernetes/


1. Checkout
Pull latest code from GitHub

Includes:

Helm charts
Terraform
Kubernetes manifests
Shell scripts
2. Build Docker Image
Creates application container

Example:

payment-app:v25
3. Login to ECR

Authenticates Docker with AWS ECR.

4. Push Docker Image

Pushes image to:

AWS Elastic Container Registry
5. Terraform Primary Infra

Creates/validates:

VPC
EKS
IAM
Subnets
Security groups
6. Connect Primary EKS
aws eks update-kubeconfig

Allows:

kubectl + helm access
7. Verify Cluster

Checks:

nodes healthy
pods healthy
8. Helm Deployment

Most real enterprises use:

Helm

Why?

templating
reusable deployments
versioning
easier upgrades
9. Rollout Verification

Ensures deployment completed successfully.

kubectl rollout status
10. Health Check

Critical stage.

Checks:

application actually working
11. DR Failover Stage

ONLY runs if:

primary region unhealthy
DR Stage Automates
Action	Purpose
Terraform DR	Create/validate DR infra
Connect DR cluster	Access DR environment
Helm deploy	Deploy app to DR
Scale deployment	Handle production traffic
Promote DB	Make DR DB writable
Switch Route53	Redirect traffic
Smoke test	Verify recovery
Real Enterprise Tools Involved
Purpose	Tool
CI/CD	Jenkins

Infrastructure	Terraform

Kubernetes	Amazon EKS

Package Manager	Helm

Monitoring	Prometheus

DNS Failover	Amazon Route 53

GitOps	Argo CD
Most Important Real Production Concept

Terraform:

Infrastructure provisioning

Helm:

Application deployment

Jenkins:

Pipeline orchestration

Monitoring:

Failure detection

Route53:

Traffic failover

Together they create:

complete enterprise DR automation










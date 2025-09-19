# DevOps Cheat Sheet

A comprehensive reference guide for DevOps tools, commands, and best practices.

## Table of Contents

1. [Docker](#docker)
2. [Kubernetes](#kubernetes)
3. [Git](#git)
4. [Linux/Unix Commands](#linuxunix-commands)
5. [CI/CD](#cicd)
6. [AWS/Cloud Services](#awscloud-services)
7. [Infrastructure as Code](#infrastructure-as-code)
8. [Monitoring & Logging](#monitoring--logging)
9. [Networking](#networking)
10. [Security](#security)

---

## Docker

### Essential Docker Commands

```bash
# Image operations
docker build -t image-name .          # Build image from Dockerfile
docker pull image-name                # Pull image from registry
docker push image-name                # Push image to registry
docker images                         # List all images
docker rmi image-id                   # Remove image

# Container operations
docker run -d --name container-name image-name  # Run container in detached mode
docker run -it image-name /bin/bash             # Run interactive container
docker run -p 8080:80 image-name               # Port mapping
docker ps                                       # List running containers
docker ps -a                                   # List all containers
docker stop container-id                       # Stop container
docker start container-id                      # Start container
docker restart container-id                    # Restart container
docker rm container-id                         # Remove container
docker exec -it container-id /bin/bash        # Execute command in running container

# System commands
docker system prune                   # Remove unused data
docker volume ls                      # List volumes
docker network ls                     # List networks
docker logs container-id              # View container logs
docker inspect container-id           # Detailed container info
```

### Docker Compose

```bash
# Basic commands
docker-compose up                     # Start services
docker-compose up -d                  # Start in detached mode
docker-compose down                   # Stop and remove containers
docker-compose build                  # Build services
docker-compose ps                     # List running services
docker-compose logs service-name      # View service logs
```

### Sample Dockerfile

```dockerfile
FROM node:16-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

---

## Kubernetes

### Essential kubectl Commands

```bash
# Cluster info
kubectl cluster-info                  # Display cluster info
kubectl get nodes                     # List nodes
kubectl describe node node-name       # Node details

# Pods
kubectl get pods                      # List pods
kubectl get pods -n namespace         # List pods in namespace
kubectl describe pod pod-name         # Pod details
kubectl logs pod-name                 # Pod logs
kubectl exec -it pod-name -- /bin/bash # Execute command in pod
kubectl delete pod pod-name           # Delete pod

# Deployments
kubectl get deployments              # List deployments
kubectl create deployment name --image=image # Create deployment
kubectl scale deployment name --replicas=3   # Scale deployment
kubectl rollout status deployment/name       # Check rollout status
kubectl rollout undo deployment/name         # Rollback deployment

# Services
kubectl get services                  # List services
kubectl expose deployment name --port=80 --type=LoadBalancer # Expose service
kubectl describe service service-name # Service details

# ConfigMaps and Secrets
kubectl create configmap name --from-file=file # Create configmap
kubectl create secret generic name --from-literal=key=value # Create secret
kubectl get configmaps               # List configmaps
kubectl get secrets                  # List secrets

# Namespaces
kubectl get namespaces               # List namespaces
kubectl create namespace name        # Create namespace
kubectl config set-context --current --namespace=name # Set default namespace

# Apply configurations
kubectl apply -f file.yaml           # Apply configuration
kubectl delete -f file.yaml          # Delete configuration
```

### Sample Kubernetes Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app:latest
        ports:
        - containerPort: 8080
```

---

## Git

### Essential Git Commands

```bash
# Repository setup
git init                             # Initialize repository
git clone url                       # Clone repository
git remote add origin url           # Add remote repository

# Basic operations
git add .                           # Stage all changes
git add file.txt                    # Stage specific file
git commit -m "message"             # Commit changes
git push origin branch-name         # Push to remote
git pull origin branch-name         # Pull from remote
git status                          # Check status
git log                             # View commit history
git log --oneline                   # Compact log view

# Branching
git branch                          # List branches
git branch branch-name              # Create branch
git checkout branch-name            # Switch branch
git checkout -b branch-name         # Create and switch branch
git merge branch-name               # Merge branch
git branch -d branch-name           # Delete branch

# Undoing changes
git reset HEAD file.txt             # Unstage file
git checkout -- file.txt            # Discard changes
git reset --hard HEAD               # Reset to last commit
git revert commit-hash              # Revert commit

# Stashing
git stash                           # Stash changes
git stash pop                       # Apply stashed changes
git stash list                      # List stashes
git stash drop                      # Delete stash
```

### Git Workflow Best Practices

```bash
# Feature branch workflow
git checkout -b feature/new-feature
# Make changes
git add .
git commit -m "Add new feature"
git push origin feature/new-feature
# Create pull request
# After approval, merge and cleanup
git checkout main
git pull origin main
git branch -d feature/new-feature
```

---

## Linux/Unix Commands

### File and Directory Operations

```bash
# Navigation
ls -la                              # List files with details
cd /path/to/directory               # Change directory
pwd                                 # Print working directory
find /path -name "*.txt"            # Find files
locate filename                     # Locate file

# File operations
cp source destination               # Copy file
mv source destination               # Move/rename file
rm file.txt                        # Remove file
rm -rf directory/                   # Remove directory recursively
mkdir directory                     # Create directory
touch filename                     # Create empty file

# File content
cat file.txt                       # Display file content
less file.txt                     # View file with pagination
head -n 10 file.txt               # First 10 lines
tail -n 10 file.txt               # Last 10 lines
tail -f file.txt                  # Follow file changes
grep "pattern" file.txt           # Search in file
grep -r "pattern" directory/      # Recursive search

# Permissions
chmod 755 file.txt                # Change permissions
chown user:group file.txt         # Change ownership
sudo command                      # Execute as root
```

### Process Management

```bash
ps aux                            # List all processes
ps aux | grep process-name        # Find specific process
kill process-id                   # Kill process
kill -9 process-id               # Force kill process
killall process-name             # Kill all processes by name
nohup command &                  # Run command in background
jobs                             # List background jobs
fg %1                            # Bring job to foreground
```

### System Monitoring

```bash
top                              # Real-time process viewer
htop                             # Enhanced process viewer
df -h                            # Disk usage
du -sh directory/                # Directory size
free -h                          # Memory usage
uptime                           # System uptime
iostat                           # I/O statistics
netstat -tulpn                   # Network connections
ss -tulpn                        # Modern netstat alternative
```

---

## CI/CD

### Jenkins Pipeline (Jenkinsfile)

```groovy
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
        
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'docker build -t myapp .'
                sh 'docker push myapp:latest'
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}
```

### GitHub Actions

```yaml
name: CI/CD Pipeline
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '16'
        
    - name: Install dependencies
      run: npm install
      
    - name: Run tests
      run: npm test
      
    - name: Build application
      run: npm run build
      
    - name: Deploy to production
      if: github.ref == 'refs/heads/main'
      run: |
        echo "Deploying to production"
```

### GitLab CI/CD

```yaml
stages:
  - build
  - test
  - deploy

build:
  stage: build
  script:
    - npm install
    - npm run build
  artifacts:
    paths:
      - dist/

test:
  stage: test
  script:
    - npm test

deploy:
  stage: deploy
  script:
    - docker build -t myapp .
    - docker push myapp:latest
  only:
    - main
```

---

## AWS/Cloud Services

### AWS CLI Commands

```bash
# Configuration
aws configure                       # Configure AWS CLI
aws sts get-caller-identity        # Verify credentials

# S3
aws s3 ls                          # List buckets
aws s3 ls s3://bucket-name         # List objects in bucket
aws s3 cp file.txt s3://bucket/    # Upload file
aws s3 cp s3://bucket/file.txt .   # Download file
aws s3 sync . s3://bucket/         # Sync directory

# EC2
aws ec2 describe-instances         # List instances
aws ec2 start-instances --instance-ids i-1234567890abcdef0
aws ec2 stop-instances --instance-ids i-1234567890abcdef0
aws ec2 create-key-pair --key-name MyKeyPair

# ECS
aws ecs list-clusters              # List clusters
aws ecs list-services --cluster cluster-name
aws ecs describe-services --cluster cluster-name --services service-name

# Lambda
aws lambda list-functions          # List functions
aws lambda invoke --function-name function-name output.json
```

### Common AWS Services

- **EC2**: Virtual servers in the cloud
- **S3**: Object storage service
- **RDS**: Managed relational database service
- **Lambda**: Serverless compute service
- **ECS/EKS**: Container orchestration services
- **CloudFormation**: Infrastructure as code
- **CloudWatch**: Monitoring and logging
- **IAM**: Identity and access management
- **VPC**: Virtual private cloud networking

---

## Infrastructure as Code

### Terraform

```hcl
# Basic AWS EC2 instance
provider "aws" {
  region = "us-west-2"
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1d0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "HelloWorld"
  }
}

output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

### Terraform Commands

```bash
terraform init                     # Initialize Terraform
terraform plan                     # Preview changes
terraform apply                    # Apply changes
terraform destroy                  # Destroy infrastructure
terraform validate                 # Validate configuration
terraform fmt                      # Format configuration files
terraform state list               # List resources in state
```

### Ansible

```yaml
# Basic playbook
---
- hosts: webservers
  become: yes
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
        
    - name: Start nginx
      service:
        name: nginx
        state: started
        enabled: yes
```

### Ansible Commands

```bash
ansible-playbook playbook.yml      # Run playbook
ansible all -m ping                # Ping all hosts
ansible all -m setup               # Gather facts
ansible-vault encrypt file.yml     # Encrypt file
ansible-vault decrypt file.yml     # Decrypt file
```

---

## Monitoring & Logging

### Prometheus Queries

```promql
# CPU usage
100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)

# Memory usage
(1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100

# Disk usage
100 - ((node_filesystem_avail_bytes * 100) / node_filesystem_size_bytes)

# HTTP request rate
rate(http_requests_total[5m])
```

### ELK Stack

```yaml
# Elasticsearch query
GET /logs/_search
{
  "query": {
    "bool": {
      "must": [
        {"range": {"@timestamp": {"gte": "now-1h"}}},
        {"match": {"level": "ERROR"}}
      ]
    }
  }
}
```

### Common Monitoring Tools

- **Prometheus**: Metrics collection and alerting
- **Grafana**: Visualization and dashboards
- **ELK Stack**: Elasticsearch, Logstash, Kibana for logging
- **Jaeger**: Distributed tracing
- **DataDog**: All-in-one monitoring platform
- **New Relic**: Application performance monitoring

---

## Networking

### Network Troubleshooting

```bash
# Connectivity
ping google.com                    # Test connectivity
ping -c 4 8.8.8.8                 # Ping with count
traceroute google.com              # Trace route
mtr google.com                     # Network diagnostic tool

# DNS
nslookup google.com                # DNS lookup
dig google.com                     # DNS lookup (detailed)
dig @8.8.8.8 google.com           # Query specific DNS server

# Ports and connections
telnet host 80                     # Test port connectivity
nc -zv host 80                     # Check port (netcat)
netstat -tulpn                     # Show listening ports
ss -tulpn                          # Modern alternative to netstat
lsof -i :80                        # Show processes using port 80

# Network configuration
ifconfig                           # Network interface configuration
ip addr show                       # Show IP addresses
ip route show                      # Show routing table
arp -a                             # Show ARP table
```

### Common Ports

- **22**: SSH
- **80**: HTTP
- **443**: HTTPS
- **21**: FTP
- **25**: SMTP
- **53**: DNS
- **3306**: MySQL
- **5432**: PostgreSQL
- **6379**: Redis
- **27017**: MongoDB

---

## Security

### SSL/TLS

```bash
# Generate private key
openssl genrsa -out private.key 2048

# Generate certificate signing request
openssl req -new -key private.key -out request.csr

# Generate self-signed certificate
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365

# Check certificate
openssl x509 -in cert.pem -text -noout

# Test SSL connection
openssl s_client -connect example.com:443
```

### Security Best Practices

#### Container Security
- Use minimal base images
- Scan images for vulnerabilities
- Don't run containers as root
- Use secrets management
- Implement resource limits

#### Infrastructure Security
- Enable encryption at rest and in transit
- Use IAM roles and policies
- Implement network segmentation
- Regular security audits and updates
- Monitor and log all activities

#### CI/CD Security
- Secure secrets in pipelines
- Implement security testing in CI/CD
- Use signed commits and images
- Implement branch protection rules
- Regular dependency updates

### Common Security Tools

- **Trivy**: Container vulnerability scanner
- **Falco**: Runtime security monitoring
- **Vault**: Secrets management
- **OWASP ZAP**: Web application security scanner
- **Nessus**: Vulnerability scanner

---

## Quick Reference

### Environment Variables

```bash
export VAR_NAME=value              # Set environment variable
echo $VAR_NAME                     # Print variable
env                                # List all environment variables
unset VAR_NAME                     # Unset variable
```

### Text Processing

```bash
sed 's/old/new/g' file.txt         # Replace text
awk '{print $1}' file.txt          # Print first column
cut -d',' -f1 file.csv             # Extract first field from CSV
sort file.txt                      # Sort lines
uniq file.txt                      # Remove duplicates
wc -l file.txt                     # Count lines
```

### Compression

```bash
tar -czf archive.tar.gz directory/ # Create compressed archive
tar -xzf archive.tar.gz            # Extract compressed archive
zip -r archive.zip directory/      # Create zip archive
unzip archive.zip                  # Extract zip archive
```

---

## Contributing

Feel free to contribute to this cheat sheet by submitting pull requests with additional commands, tools, or improvements.

## License

This cheat sheet is provided under the MIT License.
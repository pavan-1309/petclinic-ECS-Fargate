# Spring Boot Petclinic - CI/CD on AWS ECS

This repository contains the **Spring Boot Petclinic** application with a **CI/CD pipeline** powered by **GitHub Actions**, **Amazon ECR**, and **Amazon ECS (Fargate)**.

---

## 🚀 Project Overview
The [Spring Boot Petclinic](https://github.com/spring-projects/spring-petclinic) is a sample application built with:
- Java 17
- Spring Boot
- Maven

We extend it with:
- **Docker**: Containerize the app
- **Amazon ECR**: Store Docker images
- **Amazon ECS (Fargate)**: Run containers in the cloud
- **GitHub Actions**: Automate build, push, and deploy

---

## 🛠️ Run Locally

### Prerequisites
- JDK 17+
- Maven 3+
- Docker (optional for container build)

### Steps
```bash
# Clone repo
git clone https://github.com/<your-username>/spring-petclinic-ecs.git
cd spring-petclinic-ecs

# Run with Maven
mvn spring-boot:run

# Or build JAR
mvn clean package -DskipTests
java -jar target/*.jar
```

App will be available at:  
👉 [http://localhost:8080](http://localhost:8080)

---

## 🐳 Build & Run with Docker

```bash
# Build Docker image
docker build -t petclinic .

# Run container
docker run -p 8080:8080 petclinic
```

---

## ⚡ CI/CD Pipeline (GitHub Actions)

We use `.github/workflows/cicd.yml` to automate:

1. **Checkout code**
2. **Setup Java 17**
3. **Build JAR with Maven**
4. **Build Docker image**
5. **Push Docker image to Amazon ECR**
6. **Update ECS Task Definition** with new image
7. **Deploy to ECS Service**

---

## 📦 Amazon ECR & ECS Setup

1. **ECR Repository**
   - Create repo: `spring-petclinic` in **Amazon ECR**
   - Example URI: `222301802408.dkr.ecr.ap-south-1.amazonaws.com/spring-petclinic`

2. **ECS Cluster & Service**
   - Cluster: `Petclinic`
   - Service: `petclinic-service`
   - Task Definition: `petclinic`

3. **IAM Role**
   - Ensure `ecsTaskExecutionRole` has policies:
     - `AmazonECSTaskExecutionRolePolicy`
     - ECR read/write permissions
     - CloudWatch Logs permissions

---

## 🔑 GitHub Secrets

Add the following secrets in your GitHub repo:

- `AWS_ACCESS_KEY` → IAM User Access Key  
- `AWS_SECRET_ACCESS_KEY` → IAM User Secret  
- (Optional) `DOCKER_HUB_USERNAME` / `DOCKER_HUB_TOKEN` if using Docker Hub  

---

## 📄 ECS Task Definition

We maintain a template file:  
`petclinic.json`

The workflow replaces `<IMAGE_PLACEHOLDER>` with the latest Docker image from ECR before deploying.

---

## ✅ Deployment

Every push to `main` branch triggers the workflow:
- Builds → Pushes Docker image → Updates ECS service  
- Zero-downtime deployment with Fargate  

---

## 🔗 Useful Links
- [Spring Petclinic Docs](https://github.com/spring-projects/spring-petclinic)
- [GitHub Actions for AWS ECS](https://github.com/aws-actions)
- [Amazon ECS Documentation](https://docs.aws.amazon.com/ecs)

---

## 👨‍💻 Author
Built with ❤️ using Spring Boot, AWS ECS, and GitHub Actions.

# 🚀 Flask Docker App — Automated Build, Push & Email Notification (GitHub Actions + AWS SES)

This project contains a simple **Flask web application** deployed inside a Docker container.  
It also includes a **full CI/CD pipeline** using **GitHub Actions**, which:

1. Builds the Docker image
2. Pushes the image to **Docker Hub**
3. Sends **AWS SES email notifications** on build success or failure

## Project Structure

.
├── app.py               # Main Flask application
├── requirements.txt     # Python dependencies
└── .github/workflows/
       └── build.yml     # GitHub Actions workflow

## Docker Support

Your GitHub Actions workflow automatically generates the Dockerfile during the build stage:

FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt requirements.txt
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]


## 🤖 CI/CD Pipeline (GitHub Actions)

The workflow:

### ✅ Build Job

- Checks out code
- Creates Dockerfile
- Builds Docker image
- Assumes AWS IAM role
### 📤 Push Job

- Builds image again
- Pushes to Docker Hub repository:  
    **docker.io/`your-username`/akshay:latest**
### 📧 SES Notification Job

Uses **AWS SES** to send emails:
- **Success email**
- **Failure email**
- Contains:
    - Repository name
    - Branch
    - Commit SHA
    - Link to GitHub Actions run        

## ⚙️ Prerequisites

Before using this pipeline, ensure you have:
### 1️⃣ Docker Hub
Store your credentials in GitHub Secrets:

| Secret Name          | Value                    |
| -------------------- | ------------------------ |
| `DOCKERHUB_USERNAME` | Your Docker Hub username |
| `DOCKERHUB_PASSWORD` | Your Docker Hub token    |

### 2️⃣ AWS SES

- Verify your “From” email in SES    
- Configure IAM Role for GitHub OIDC
- Add AWS SES permissions (SendEmail, etc.)

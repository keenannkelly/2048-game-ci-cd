# 2048 Game CI/CD Pipeline

A containerized 2048 game with automated CI/CD deployment on AWS using ECS, ECR, and CodeBuild.

## 🏗️ Architecture

This project demonstrates a complete CI/CD pipeline using AWS services:

- **GitHub**: Source code repository
- **AWS CodeBuild**: Automated build and Docker image creation
- **Amazon ECR**: Docker image registry
- **Amazon ECS**: Container orchestration and deployment
- **Docker**: Application containerization

## 📁 Project Structure

```
├── index.html          # Main game HTML file
├── Dockerfile          # Container configuration
├── buildspec.yml       # CodeBuild configuration
├── js/                 # Game JavaScript files
├── style/              # CSS and font files
├── meta/               # App icons and metadata
└── README.md           # This file
```

## 🚀 Deployment Pipeline

1. **Code Push**: Developer pushes changes to GitHub repository
2. **Trigger Build**: CodeBuild automatically detects changes
3. **Build Image**: Docker image built with `--platform linux/amd64`
4. **Push to ECR**: Image pushed to Amazon ECR repository
5. **Deploy to ECS**: ECS service automatically deploys new image
6. **Live Update**: Application accessible via new public IP

## 📊 Monitoring

- **CodeBuild Logs**: Available in AWS CodeBuild console
- **ECS Logs**: Available in CloudWatch Logs
- **Application Metrics**: ECS service metrics in CloudWatch

---

*Built with ❤️ using AWS services and modern DevOps practices*

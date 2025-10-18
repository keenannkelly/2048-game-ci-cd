# 2048 Game CI/CD Pipeline

A containerized 2048 game with automated CI/CD deployment on AWS using ECS, ECR, and CodeBuild.

## 🎮 Live Demo

The game is deployed and accessible at: `http://[current-ecs-task-ip]`

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

## 🛠️ AWS Resources

### CodeBuild Project
- **Name**: `2048-game-build`
- **Source**: GitHub repository
- **Build Environment**: Standard Linux container

### ECR Repository
- **Name**: `2048-game-repo`
- **Region**: `us-east-2`
- **URI**: `762233738167.dkr.ecr.us-east-2.amazonaws.com/2048-game-repo`

### ECS Configuration
- **Cluster**: `keenan-2048-game-cluster`
- **Service**: `2048-service`
- **Task Definition**: `2048-game-task`
- **Container**: `2048-container`

## 🔧 Local Development

### Prerequisites
- Docker installed
- AWS CLI configured
- Git

### Build Locally
```bash
# Clone the repository
git clone https://github.com/keenannkelly/2048-game-ci-cd.git
cd 2048-game-ci-cd

# Build Docker image
docker build --platform linux/amd64 -t 2048-game .

# Run locally
docker run -p 8080:80 2048-game
```

Access the game at `http://localhost:8080`

## 📋 BuildSpec Configuration

The `buildspec.yml` file defines the CI/CD pipeline:

```yaml
version: 0.2
phases:
  pre_build:
    commands:
      - echo Logging in to Amazon ECR...
      - aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 762233738167.dkr.ecr.us-east-2.amazonaws.com
  build:
    commands:
      - echo Building the Docker image...
      - docker build --platform linux/amd64 -t 2048-game .
      - docker tag 2048-game:latest 762233738167.dkr.ecr.us-east-2.amazonaws.com/2048-game-repo:latest
  post_build:
    commands:
      - echo Pushing the Docker image to Amazon ECR...
      - docker push 762233738167.dkr.ecr.us-east-2.amazonaws.com/2048-game-repo:latest
      - echo Creating imagedefinitions.json file for ECS deployment...
      - echo '[{"name":"2048-container","imageUri":"762233738167.dkr.ecr.us-east-2.amazonaws.com/2048-game-repo:latest"}]' > imagedefinitions.json
artifacts:
  files:
    - imagedefinitions.json
```

## 🔐 Security Configuration

### Security Group Rules
- **Inbound**: Port 80 (HTTP) from 0.0.0.0/0
- **Outbound**: All traffic allowed

### IAM Permissions
Required permissions for CodeBuild service role:
- ECR push/pull access
- ECS task execution
- CloudWatch logs

## 🎯 Features

- **Responsive Design**: Works on desktop and mobile devices
- **Automated Deployment**: Zero-downtime deployments
- **Container Security**: Runs on nginx base image
- **Platform Compatibility**: Built for AMD64 architecture
- **Monitoring**: CloudWatch integration for logs and metrics

## 🔄 Making Changes

1. Edit source files locally
2. Commit and push to GitHub:
   ```bash
   git add .
   git commit -m "Your change description"
   git push origin main
   ```
3. Monitor CodeBuild for automatic deployment
4. Access updated application at new ECS task IP

## 📊 Monitoring

- **CodeBuild Logs**: Available in AWS CodeBuild console
- **ECS Logs**: Available in CloudWatch Logs
- **Application Metrics**: ECS service metrics in CloudWatch

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test locally with Docker
5. Submit a pull request

## 📝 License

This project is open source and available under the MIT License.

## 👨‍💻 Author

**Keenan Kelly**

- GitHub: [@keenannkelly](https://github.com/keenannkelly)
- Project: [2048 Game CI/CD](https://github.com/keenannkelly/2048-game-ci-cd)

---

*Built with ❤️ using AWS services and modern DevOps practices*
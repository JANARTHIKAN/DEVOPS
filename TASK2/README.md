## 📸 **Screenshots**
### 1️⃣ Docker Hub Uploaded Image  
The Docker image is successfully built and pushed to Docker Hub.

![Docker Hub Image](https://github.com/user-attachments/assets/ca7d91a9-ec6c-46dd-8003-5a5e39c0cfed)

---

### 2️⃣ Task-2 Completion Output  
The pipeline execution output shows the successful completion of Task-2, including the image build and deployment.

![Task 2 Output](https://github.com/user-attachments/assets/6429aae7-5766-4757-8c8f-397e6e5d5a8e)

---

### 3️⃣ Jenkins Pipeline Code  
Below is the pipeline script used for building and deploying the application.

```groovy
pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
    }
    
    stages {
        stage('SCM Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/lily4499/lil-node-app.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t username/lil-node-app:latest .'
            }
        }
        
        stage('Login to Docker Hub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        
        stage('Push Image to Docker Hub') {
            steps {
                sh 'docker push username/lil-node-app:latest'
            }
        }
        
        stage('Deploy Container') {
            steps {
                sh 'docker run -d -p 3000:3000 --name lil-node-app username/lil-node-app:latest'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

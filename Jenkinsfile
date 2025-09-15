pipeline {
    agent any

    environment {
        REGISTRY = "docker.io"
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        FRONTEND_IMAGE = "vaindrum/upcast-frontend"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Vaindrum/UpCast'
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh """
                    docker build \
                    --build-arg NEXT_PUBLIC_BACKEND_URL=http://upcast-backend:5000 \
                    -t $FRONTEND_IMAGE:latest .
                """
            }
        }

        stage('Login & Push') {
            steps {
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                sh "docker push $FRONTEND_IMAGE:latest"
            }
        }
    }
}

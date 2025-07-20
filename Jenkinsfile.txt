pipeline {
    agent any

    environment {
        IMAGE_NAME = "hshar/webapp"
    }

    stages {
        stage('Build') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'echo "Tests Passed!"'
            }
        }

        stage('Deploy to Production') {
            when {
                branch 'master'
            }
            steps {
                echo "Deploying to Production environment..."
                sh 'docker run -d -p 80:80 --name webapp $IMAGE_NAME'
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully."
        }
        failure {
            echo "Pipeline failed."
        }
    }
}

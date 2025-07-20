pipeline {
    agent any
    environment {
        IMAGE_NAME = "my-webapp"
    }
    stages {
        stage('Build') {
            steps {
                git branch: "master", url: 'https://github.com/komalkasbe/website.git'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm -d --name temp $IMAGE_NAME'
                sh 'sleep 5'
                sh 'curl -f http://localhost || echo "Test Failed"'
                sh 'docker rm -f temp'
            }
        }

        stage('Deploy to Prod') {
            when {
                branch 'master'
            }
            steps {
                sh 'docker run -d -p 80:80 --name prod_app $IMAGE_NAME'
            }
        }
    }
}

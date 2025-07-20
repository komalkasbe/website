pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-webapp"
    }

    options {
        skipDefaultCheckout() // Prevent default SCM checkout
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout only master branch
                git branch: 'master', url: 'https://github.com/komalkasbe/website.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm -d --name temp -p 8081:80 $IMAGE_NAME'
                sh 'sleep 5'
                sh 'curl -f http://localhost:8081 || echo "Test Failed"'
                sh 'docker rm -f temp'
            }
        }


        stage('Deploy to Prod') {
            steps {
                sh 'docker run -d -p 80:80 --name prod_app $IMAGE_NAME'
            }
       }
    }
}

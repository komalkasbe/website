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
                sh 'docker run --rm -d --name temp -p 8080:80 $IMAGE_ĀNAME'
                sh '''
                    echo "Waiting for app to be ready..."
                    for i in {1..10}; do
                      if curl -f http://localhost:8080; then
                        echo "App is up!"
                        break
                      fi
                      echo "Retry $i/10: App not yet available, sleeping..."
                      sleep 2
                    done
                '''
                sh 'docker rm -f temp || true'
            }
        }


        stage('Deploy to Prod') {
            steps {
                sh 'docker run -d -p 80:80 --name prod_app $IMAGE_NAME'
            }
       }
    }
}

pipeline {
    agent any

    stages {

        stage('Code Quality Analysis') {
            steps {
                // Run code quality analysis using SonarQube
                sh 'sonar-scanner -Dsonar.projectKey=flask-hello -Dsonar.sources=.'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image for the application
                sh 'docker build -t kdradgzl/flask-hello:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                // Build the Docker image for the application
                sh 'docker push kdradgzl/flask-hello:latest'
            }
        }

        //     stage('Deploy to Kubernetes') {
        //     steps {
        //         // Deploy the application to Kubernetes using Helm
        //         sh 'helm upgrade --install flask-hello ./helm-chart --set image.tag=latest'
        //     }
        // }
    }

    post {
        always {
            // Print a message when the pipeline is finished
            echo "Pipeline completed."
        }
    }
}


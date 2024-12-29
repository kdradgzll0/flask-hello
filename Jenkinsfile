pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // GitHub repository URL'si ve branch: 'main'
                git branch: 'main', url: 'https://github.com/kdradgzll0/flask-hello.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install required Python libraries
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Code Quality Analysis') {
            steps {
                // Run code quality analysis using SonarQube
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner -Dsonar.projectKey=flask-hello -Dsonar.sources=.'
                }
            }
        }

        stage('Quality Gate Check') {
            steps {
                // Check if the code passes the quality gate; fail if it doesn't
                script {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline failed due to Quality Gate failure: ${qg.status}"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image for the application
                sh 'docker build -t flask-hello .'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Deploy the application to Kubernetes using Helm
                sh 'helm upgrade --install flask-hello ./helm-chart --set image.tag=latest'
            }
        }
    }

    post {
        always {
            // Print a message when the pipeline is finished
            echo "Pipeline completed."
        }
    }
}


pipeline {
    agent any

    environment {
        // Docker registry credentials
        IMAGE = ""
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout application repository
                git credentialsId: '6351342b-e3a4-4600-bda7-1328ed9edd3c', url: 'https://github.com/Volodya3200/application.git'
            }
        }

        stage('Parse File') {
            steps {
                // Checkout scripts repository to use the script
                sh 'git clone https://github.com/Volodya3200/scripts.git'

                // Run the parse script to extract Docker image name
                script {
                    IMAGE = sh(script: './scripts/parse.sh', returnStdout: true).trim()
                }
                echo "Parsed Docker Image: ${IMAGE}"
            }
        }

        stage('Build') {
            steps {
                script {
                    echo "Building Docker image ${IMAGE}"

                    // Build Docker image
                    sh "docker build -t ${IMAGE} ."
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    echo "Pushing Docker image ${IMAGE}"

                    sh "docker login -u vvzlssk32 -p WSZ1399tyv347"
                    sh "docker push ${IMAGE}"
                }
            }
        }

        stage('Pull') {
            steps {
                script {
                    echo "Pulling Docker image ${IMAGE}"

                    sh "docker pull ${IMAGE}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying Docker container ${IMAGE}"
                    sh "docker run -d -p 127.0.0.2:80:80 ${IMAGE}"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}


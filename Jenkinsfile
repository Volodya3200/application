pipeline {
    agent any

    environment {
        IMAGE = ""
    }

    stages {
       
        stage('Checkout') {
            steps {
                git credentialsId: '163ec2a9-8a36-4ea0-bdb7-1698a9d137ad', url: 'https://github.com/Volodya3200/application.git', branch: 'main'
            }
        }

        stage('Parse File') {
            steps {
                script {
                    if (fileExists('scripts')) {
                        dir('scripts') {
                            sh 'git pull origin main'
                        }
                    } else {
                        sh 'git clone https://github.com/Volodya3200/scripts.git'
                    }
                }

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


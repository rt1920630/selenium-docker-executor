pipeline {
    agent any
    environment {
        HUB_HOST = "172.18.0.2"  // Replace with the actual IP address of the Selenium Hub
    }
    stages {
        stage("Checkout") {
            steps {
                checkout scm
            }
        }
        stage("Pull Latest Image") {
            steps {
                bat "docker pull rt536/selenium-docker"
            }
        }
        stage("Start Grid") {
            steps {
                bat "docker-compose up -d hub chrome firefox edge"
            }
        }
        stage("Run Test") {
            steps {
                script {
                    try {
                        bat "docker-compose up search_module"
                    } catch (Exception e) {
                        echo "Test failed: ${e.message}"
                    }
                }
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**'
            bat "docker-compose down"
        }
    }
}

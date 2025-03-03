pipeline {
    agent any
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
                bat "docker-compose up -d hub chrome firefox"
            }
        }
        stage("Run Test") {
            steps {
                script {
                    try {
                        bat "docker-compose up search-module"
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

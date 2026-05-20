```groovy
pipeline {
    agent any
    
    environment {
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_USERNAME = credentials('docker-hub-creds-username')
        DOCKER_PASSWORD = credentials('docker-hub-creds-password')
        SONARCLOUD_TOKEN = credentials('sonarcloud-token')
        GIT_CREDENTIALS = credentials('github-credentials')
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo '=========================================='
                echo 'Stage 1: Cloning Repository'
                echo '=========================================='
                checkout scm
            }
        }
        
        stage('SonarCloud Analysis') {
            steps {
                echo '=========================================='
                echo 'Stage 2: Running SonarCloud Analysis'
                echo '=========================================='
                withSonarQubeEnv('SonarCloud') {
                    bat '''
                        sonar-scanner.bat ^
                            -Dsonar.projectKey=SudhanshuKumar-4220_DevOps-Lab ^
                            -Dsonar.organization=sudhanshukumar-4220 ^
                            -Dsonar.sources=. ^
                            -Dsonar.host.url=https://sonarcloud.io
                    '''
                }
            }
        }
        
        stage('Build Docker Images') {
            steps {
                echo '=========================================='
                echo 'Stage 3: Building Docker Images'
                echo '=========================================='
                bat '''
                    cd DevOps-Lab
                    docker build -t sudhanshu4220/inkwell-backend:latest ./server
                    docker build -t sudhanshu4220/inkwell-frontend:latest ./client
                    echo Backend and Frontend images built successfully!
                '''
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                echo '=========================================='
                echo 'Stage 4: Pushing Images to Docker Hub'
                echo '=========================================='
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                        docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                        cd DevOps-Lab
                        docker push sudhanshu4220/inkwell-backend:latest
                        docker push sudhanshu4220/inkwell-frontend:latest
                        docker logout
                        echo Images pushed to Docker Hub successfully!
                    '''
                }
            }
        }
    }
    
    post {
        always {
            echo '=========================================='
            echo 'Pipeline Execution Complete'
            echo '=========================================='
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check logs above.'
        }
    }
}
```
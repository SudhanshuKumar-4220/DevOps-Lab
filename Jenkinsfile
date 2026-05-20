pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                echo '=========================================='
                echo 'Stage 1: Cloning Repository'
                echo '=========================================='
                checkout scm
            }
        }
        
        stage('Build Docker Images') {
            steps {
                echo '=========================================='
                echo 'Stage 2: Building Docker Images'
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
                echo 'Stage 3: Pushing Images to Docker Hub'
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
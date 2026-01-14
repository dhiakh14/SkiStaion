pipeline {
    agent any
    
    environment {
        SONARQUBE_SERVER = 'SonarQube'
        DOCKER_IMAGE_NAME = 'mddhia/gestion-station-ski'
        DOCKER_IMAGE_TAG  = 'latest'
    }
    
    tools {
        maven 'M2_HOME'   // Make sure Maven is configured in Jenkins global tools
    }
    
    stages {
        // 1️⃣ Git Checkout
        stage('Git') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[ url: 'https://github.com/dhiakh14/SkiStaion.git' ]]
                ])
            }
        }
        
        // 2️⃣ Maven Build
        stage('Maven Build') {
            steps {
                bat 'mvn clean install'
            }
        }
        
        // 3️⃣ Run Tests
        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }
        
        // 4️⃣ SonarQube Analysis
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn sonar:sonar'
                }
            }
        }
        
        // 5️⃣ Check JAR exists
        stage('Check JAR') {
            steps {
                bat 'if not exist target\\gestion-station-ski-1.0.jar exit /b 1'
            }
        }
        
        // 6️⃣ Build Docker Image
        stage('Build Docker Image') {
            steps {
                script {
                    // Debug workspace
                    bat 'echo Contenu du workspace actuel :'
                    bat 'cd'
                    bat 'dir /s'

                    // Build the Docker image
                    bat "docker build -t %DOCKER_IMAGE_NAME%:%DOCKER_IMAGE_TAG% -f Dockerfile ."
                }
            }
        }
        
        // 7️⃣ Push Docker Image to DockerHub
        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'docker-hub-credentials',   // Your Jenkins credentials ID
                        usernameVariable: 'DOCKERHUB_USERNAME',
                        passwordVariable: 'DOCKERHUB_PASSWORD'
                    )]) {
                        // Login to DockerHub using PAT
                        bat "docker login -u %DOCKERHUB_USERNAME% -p %DOCKERHUB_PASSWORD%"
                        
                        // Push image
                        bat "docker push %DOCKER_IMAGE_NAME%:%DOCKER_IMAGE_TAG%"
                    }
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline finished'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check the logs.'
        }
    }
}

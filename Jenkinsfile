pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube'  // Nom du serveur SonarQube configuré dans Jenkins
    }

    stages {
        // 1️⃣ Stage Git : récupérer le code depuis GitHub
        stage('Git') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/dhiakh14/SkiStaion.git'
                    ]]
                ])
            }
        }

        // 2️⃣ Stage Maven Build : compiler le projet
        stage('Maven Build') {
            steps {
                bat 'mvn clean install'
            }
        }

        // 3️⃣ Stage Test : lancer les tests
        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        // 4️⃣ Stage SonarQube Analysis
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn sonar:sonar'
                }
            }
        }
    }
}

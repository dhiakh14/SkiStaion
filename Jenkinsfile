pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube'  // Nom du serveur SonarQube configuré dans Jenkins
    }

    stages {
        // 1️⃣ Stage Git : Récupérer le code depuis Git
        stage('Git') {
            steps {
                script {
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/main']],  // Branche correcte
                        userRemoteConfigs: [[
                            url: 'https://github.com/dhiakh14/SkiStaion.git'
                        ]]
                    ])
                }
            }
        }

        // 2️⃣ Stage Maven Build : Build du projet avec Maven
        stage('Maven Build') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        }

        // 3️⃣ Stage Test : Lancer les tests Maven
        stage('Test') {
            steps {
                script {
                    sh 'mvn test'
                }
            }
        }

    } // <-- fermeture du bloc stages
} // <-- fermeture du bloc pipeline

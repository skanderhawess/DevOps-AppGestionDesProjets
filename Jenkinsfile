pipeline {
    agent any

    // Noms à créer dans : Administrer Jenkins > Tools
    tools {
        jdk   'JDK21'
        maven 'M3'
    }

    // Bonus : build déclenché par le webhook GitHub
    triggers {
        githubPush()
    }

    environment {
        // Fork de skanderhawess
        REPO_URL = 'https://github.com/skanderhawess/DevOps-AppGestionDesProjets.git'
    }

    stages {
        stage('Checkout GIT') {
            steps {
                echo 'Récupération du code depuis GitHub...'
                git branch: 'main',
                    url: "${REPO_URL}",
                    credentialsId: 'github-credentials'
            }
        }

        stage('Build Maven') {
            steps {
                // Le pom.xml du projet Spring se trouve dans le dossier backend/
                dir('backend') {
                    sh 'mvn -v'
                    sh 'mvn clean package -DskipTests'
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'backend/target/*.jar', fingerprint: true
            echo 'Build réussi : le .jar est dans backend/target/'
        }
        failure {
            echo 'Le build a échoué, voir la Console Output.'
        }
    }
}

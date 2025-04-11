pipeline {
    agent any

    environment {
        SPRING_PROFILES_ACTIVE = 'dev'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Barhoum1919/foyer_devops.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }
stage('MVN SONARQUBE') {
    steps {
        withSonarQubeEnv('MySonarQubeServer') {
            sh 'mvn clean verify sonar:sonar '
        }
    }
}
    }

    post {
        success {
            echo ' Build succeeded!'
        }
        failure {
            echo ' Build failed.'
        }
    }
}


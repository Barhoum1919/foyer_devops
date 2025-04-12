pipeline {
    agent any

    environment {
        SPRING_PROFILES_ACTIVE = 'dev'
        SONAR_HOST_URL = 'http://localhost:9000'       
        SONAR_TOKEN = credentials('sonar-token')  
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
   stage('SonarQube Analysis') {
            steps {
                sh """
                    mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=foyer-devops \
                    -Dsonar.projectName='Foyer DevOps' \
                    -Dsonar.host.url=$SONAR_HOST_URL \
                    -Dsonar.login=$SONAR_TOKEN
                """
            }
        }
 stage('Deploy to Nexus') {
    steps {
        sh 'mvn deploy'
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


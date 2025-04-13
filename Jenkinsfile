pipeline {
    agent any

    environment {
               SPRING_PROFILES_ACTIVE = 'dev'
               SONAR_HOST_URL = 'http://localhost:9000'
               SONAR_TOKEN = credentials('sonar-token')
               DOCKERHUB_USERNAME = 'ibrahimdarghouthi1919'
               DOCKERHUB_PASSWORD = credentials('dockerhub-password')
               IMAGE_NAME = 'foyer-devops'
               VERSION = 'latest'
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

        stage('Build Docker Image with Docker Compose') {
            steps {
                script {

                    sh """
                        docker-compose -f docker-compose.yml build app
                    """
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {

                    sh """
                        echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin
                    """
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {

                    sh """
                        docker tag foyer-devops_app:latest $DOCKERHUB_USERNAME/$IMAGE_NAME:$VERSION
                    """
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {

                    sh """
                        docker push $DOCKERHUB_USERNAME/$IMAGE_NAME:$VERSION
                    """
                }
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


/* Requires the Docker Pipeline plugin */
pipeline {
    agent {
        docker {
            image 'maven:3.8.7-eclipse-temurin-11' 
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Compile') {
            steps {
                sh './mvnw package'
            }
        }
        stage('Test') { 
            steps {
                sh './mvnw test'
            }
        }
        stage('Build Docker Image') { 
            steps {
                sh './mvnw spring-boot:build-image'
            }
        }
    }
}
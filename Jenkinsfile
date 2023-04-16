//Declarative Pipeline
def VERSION='1.0.0'
pipeline {
    agent none
    // tools {
	//  maven 'apache-maven-3.6.3'
    // }
    environment {
        PROJECT = "WELCOME TO DEVOPS b30 BATCH - Jenkins Class"
        AZ_SUB_ID = "9ce91e05-4b9e-4a42-95c1-4385c54920c6"
        AZ_TEN_ID = "2b387c91-acd6-4c88-a6aa-c92a96cab8b1"
    }
    stages {
        stage("Dev Tools Verification") {
            agent { label 'DEV' }
            steps {
                sh "mvn --version"
                sh "java --version"
            }
        }
        stage('Dev Sonarqube SAST') {
            agent { label 'DEV' }
            steps { 
                withSonarQubeEnv('DevOpsB30-SonarQube'){
                     sh "mvn clean verify sonar:sonar \
                     -Dsonar.projectKey=spring-boot-app-dev \
                     -Dsonar.projectName=spring-boot-app-dev \
                     -Dsonar.host.url=http://ec2-44-195-60-191.compute-1.amazonaws.com:9000"
                }

            }
        }
        stage("Dev Quality gate") {
            when {
                branch 'Development'
            }
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
    }
}
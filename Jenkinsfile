pipeline{

    agent any
    stages{
        stage('Git Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Mark-1305/demo-counter-app.git'
            }
        }
        stage('Unit Testing'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Integration Test'){
            steps{
                sh 'mvn verify -DskipUnitTests '
            }
        }
        stage('Maven Build'){
            steps{
                sh 'mvn clean install'
            }
        }   
        stage('Static Code Analysis'){
            steps{
                withSonarQubeEnv(credentialsId: 'sonar-api', installationName: 'sonarqube') {
                    sh "mvn clean package sonar:sonar"
                }
            }
        }  
        stage('Quality Gate Status'){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                }
                
            }
        }                      
    }
}

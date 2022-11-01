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
                script{
                withSonarQubeEnv(credentialsId: 'sonar-api') {
                    sh "mvn clean package sonar:sonar"
                    }                    
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
        stage('Upload jar file to Nexus'){
            steps{
                script{
                    def readpomVersion = readMavenPom file: 'pom.xml'
                    nexusArtifactUploader artifacts: 
                    [
                        [artifactId: 'springboot', 
                        classifier: '', 
                        file: 'target/Uber.jar', 
                        type: 'jar']
                        ], 

                    credentialsId: 'nexus-auth', 
                    groupId: 'com.example', 
                    nexusUrl: '10.0.8.74:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'demo-app-release', 
                    version: "${readpomVersion.version}"

                }
            }
        }        
    }
}

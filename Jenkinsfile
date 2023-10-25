pipeline {

    agent any
    tools{
        maven 'maven3'
    }

    stages{
       stage('GitCheckout'){
          steps{
             git branch: 'main', changelog: false, poll: false, url: 'https://github.com/balu1123/demo-counter-app.git'
          } 
       }  

       stage('UNIT TESTING'){
           steps{
              sh 'mvn test'
           }  
       }

       stage('Integration Testing'){
           steps{
              sh 'mvn verify -DskipUnitTests'
           }
       }   

       stage('Maven Build'){
           steps{
              sh 'mvn clean install'
           }
       }   

       stage('Sonar code quality Check'){
           steps{
             script{
                 withSonarQubeEnv(credentialsId: 'sonar_cred') {
                    sh 'mvn clean package sonar:sonar'
                 }   
              }
           }
       }

       stage('Quality Gate Status'){
           steps{
             script{
                 waitForQualityGate abortPipeline: false, credentialsId: 'sonar_cred'
              }
           }
       }  

       stage('Upload Artifact to Nexus'){
           steps{
             script{

                def pom = readMavenPom file: 'pom.xml'

                def nexusRepo = pom.version.endsWith("SNAPSHOT") ? "demoapp-SNAPSHOT" : "demoapp-release"

                 nexusArtifactUploader artifacts: [
                    [artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']
                    ],
                  credentialsId: 'nexus', 
                  groupId: 'com.example', 
                  nexusUrl: '192.168.9.147:8081', 
                  nexusVersion: 'nexus3', 
                  protocol: 'http', 
                  repository: nexusRepo, 
                  version: "${pom.version}"
              }
           }
       } 

       stage('OWASP Dependency Check'){
           steps{
             script{
                 dependencyCheck additionalArguments: '', odcInstallation: 'DP'
                 dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
              }
           }
       }  

       stage('Docker Image Build'){
          steps{
            script{
                sh 'sudo chmod 666 /var/run/docker.sock'
                sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                sh 'docker image tag $JOB_NAME:v1.$BUILD_ID bbaludevops/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker image tag $JOB_NAME:v1.$BUILD_ID bbaludevops/$JOB_NAME:latest'
            }
          }
       } 

       stage('Push Image to the DockerHub'){
          steps{
            script{
                withCredentials([string(credentialsId: 'docker', variable: 'docker_hub')]) {
                     
                     sh 'docker login -u bbaludevops -p ${docker_hub}'
                     sh 'docker image push bbaludevops/$JOB_NAME:v1.$BUILD_ID'
                     sh 'docker image push bbaludevops/$JOB_NAME:latest'
                }                
            }
          }
       } 
      
       stage('Trivy Scanner for Docker image'){
           steps{
             script{
                 sh "trivy image bbaludevops/counter-demo:latest"
              }
           }
       }  

    }
}

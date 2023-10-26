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

       stage('OWASP Dependency Check'){
           steps{
             script{
                 dependencyCheck additionalArguments: '', odcInstallation: 'DP'
                 dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
              }
           }
       }  

       stage('Nexus Artifact Uploader'){
           steps{
             script{
               
                 nexusArtifactUploader artifacts: [
                  [artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']
                  ], 
                  credentialsId: 'nexus_cred', 
                  groupId: 'com.example', 
                  nexusUrl: '192.168.1.171:8081', 
                  nexusVersion: 'nexus3', 
                  protocol: 'http', 
                  repository: 'testdemo-SNAPSHOT', 
                  version: '2.0.0'
                 
              }
           }
       }  
           
   }
}   
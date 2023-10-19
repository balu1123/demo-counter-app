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
              withSonarQubeEnv(credentialsId: 'sonar_cred') {
                sh 'mvn clean package sonar:sonar'
              }
           }
       }   
    }
}

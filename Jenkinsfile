pipeline {

    agent any
	tools{
           maven 'maven3'		
	}

	environment {
        SCANNER_HOME= tool 'sonar-scanner'
    }
         
    stages{
       stage('Git Checkout'){
           steps{
              git branch: 'main', changelog: false, poll: false, url: 'https://github.com/balu1123/demo-counter-app.git'
	   }
       }

        stage('UNIT Testing'){
           steps{
	      sh 'mvn test'
           }
       }

	stage('Integrated Unit Testing'){
           steps{
	       sh 'mvn verify -DskipUnitTests'
           }
       }    

	stage('Maven Building'){
           steps{
	       sh 'mvn clean install'
           }
       }   
	 stage('Sonarqube Analysis'){
             steps{
		scripts{     
		     withSonarQubeEnv(credentialsId: 'sonar'){
                     sh 'mvn clean package sonar:sonar'
		   }	
                }     
	     }		 
	 }   

    }

}












































































































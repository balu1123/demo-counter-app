pipeline {

    agent any
	tools{
           maven 'maven3'		
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
		script{     
		     withSonarQubeEnv(credentialsId: 'sonar'){
                     sh 'mvn clean package sonar:sonar'
		   }	
                }     
	     }		 
	 }   
          stage('Quality gate Status'){
             steps{
		script{     
		     waitForQualityGate abortPipeline: false, credentialsId: 'sonar'
                }     
	     }		 
	 }  
        stage('Upload War file to Nexus'){
             steps{
		script{     
		     nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/uber.jar', type: 'jar']], 
			     credentialsId: 'nexus-cred', 
			     groupId: 'com.example', 
			     nexusUrl: '54.87.54.102:8081/', 
			     nexusVersion: 'nexus3', 
			     protocol: 'http', 
			     repository: 'demoapp-release', 
			     version: '1.0.0'
                }     
	     }		 
	 }     
	    
    }

}












































































































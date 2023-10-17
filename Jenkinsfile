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
			
		     def readPomVersion = readMavenPom file: 'pom.xml'	
			
		     def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "demoapp-snapshot" : "demoapp-release"
			
		     nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']], 
			     credentialsId: 'nexus-cred', 
			     groupId: 'com.example', 
			     nexusUrl: '3.81.222.92:8081/', 
			     nexusVersion: 'nexus3', 
			     protocol: 'http', 
			     repository: 'nexusRepo', 
			     version: "${readPomVersion.version}"
                }     
	     }		 
	 }     
	    
    }

}












































































































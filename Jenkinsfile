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
			     nexusUrl: '34.230.2.10:8081/', 
			     nexusVersion: 'nexus3', 
			     protocol: 'http', 
			     repository: nexusRepo, 
			     version: "${readPomVersion.version}"
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
	 stage('Push Image to DockerHub'){
	    steps{
	       script{
		   withCredentials([string(credentialsId: 'dock-cred', variable: 'docker-hub-cred')]) {
                           sh 'docker login -u bbaludevops -p ${docker-hub-cred}'
			   sh 'docker image push bbaludevops/$JOB_NAME:v1.$BUILD_ID'
			   sh 'docker image push bbaludevops/$JOB_NAME:latest'
                  }    
	       }	    
	    } 	 
	 }   
	    
    }

}












































































































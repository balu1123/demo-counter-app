pipeline{
	agent any
	tools{
	   maven 'maven'
	}

	stages{
	   stage("Git Checkout"){
		 steps{
			script{
				git branch: 'main', changelog: false, poll: false, url: 'https://github.com/balu1123/demo-counter-app.git'
			}
		 }
	   }	

	   stage("UNIT Testing"){
		  steps{
			 sh 'mvn test'
		  }
	   }

	   stage("Integration Testing"){
		  steps{
			sh 'mvn verify -DskipUnitTest'
		  }
	   }

	   stage("Maven build"){
		steps{
			sh 'mvn clean install'
		      }
	    }

		stage("Sonarqube Analysis"){
			steps{
				script{
					withSonarQubeEnv(credentialsId: 'sonar') {
                     sh 'mvn clean package sonar:sonar'
                   }
				}
			}
		}

		stage("Quality Gates"){
		   steps{
			 script{
				waitForQualityGate abortPipeline: false, credentialsId: 'sonar'
			 }
		   }
		}

		stage("OWASP Dependency"){
		   steps{
			  script{
				dependencyCheck additionalArguments: '', odcInstallation: 'DP'
				dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
			  }
			}
		}

		stage("Nexus Artifact upload"){
	       steps{
		        script{
			
		        nexusArtifactUploader artifacts: [
				    [artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']
				    ], 
				    credentialsId: 'nexus', 
				    groupId: 'com.example', 
				    nexusUrl: '35.175.240.203:8081', 
				    nexusVersion: 'nexus3', 
				    protocol: 'http', 
				    repository: 'demoapp-release', 
				    version: "3.0.0"
		        }  
		    }
	    }
	}
}
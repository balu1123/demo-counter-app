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
		  sh 'mvn verify -DskipUnitTests'
		  }
	  }
	  
	  stage("Maven Build"){
	    steps{
		  sh 'mvn clean install'
		  }
	  }

    stage("Sonarqube Code Analysis"){
	    steps{
		   script{
		     withSonarQubeEnv(credentialsId: 'sonar'){
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
		  
		    def pom = readMavenpom file: 'pom.xml'
			def nexusRepo = pom.version.endwith("SNAPSHOT") ? "demoapp-SNAPSHOT" : "demoapp-release"
			
		    nexusArtifactUploader artifacts: [
                  [artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']
                  ], 
                  credentialsId: 'nexus', 
                  groupId: 'com.example', 
                  nexusUrl: '192.168.1.171:8081', 
                  nexusVersion: 'nexus3', 
                  protocol: 'http', 
                  repository: nexusRepo, 
                  version: "${pom.version}"
		   }  
		 }
	  }

  }
}     
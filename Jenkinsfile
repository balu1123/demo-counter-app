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
		    waitForQualityGate abortPipeline: false, credentialsId: 'sonar_cred'
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

  }
}     
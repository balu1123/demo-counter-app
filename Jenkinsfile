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
	      script{   
                  sh 'mvn test'
	      }      
           }
       }

	stage('Integrated Unit Testing'){
           steps{
	       sh 'mvn verify -DskipUnitTests'
           }
       }    

    }

}




























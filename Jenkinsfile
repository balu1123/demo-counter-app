pipeline {

    agent any
	tools{
           maven=maven		
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

    }

}

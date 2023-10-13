pipeline {

    agent any
    tools{
       jdk 'jdk17'
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

    }

}

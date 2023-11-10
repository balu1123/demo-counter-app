pipeline{
    agent any

    tools{
        maven 'maven'
    }

    stages{
      stage("Git Checkout"){
         steps{
            sh 'git branch: 'main', changelog: false, poll: false, url: 'https://github.com/balu1123/demo-counter-app.git''
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
    }
}
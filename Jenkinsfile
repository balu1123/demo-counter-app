pipline {

    agent any
    tools{
        maven 'maven3'
    }

    stages{
       stage('GitCheckout'){
          steps{
             git branch: 'main', changelog: false, poll: false, url: 'https://github.com/balu1123/demo-counter-app.git'
          } 
       }  
    }
}

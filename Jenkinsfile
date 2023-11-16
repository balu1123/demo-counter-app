pipeline{
    agent any

    tools{
        maven 'maven'
    }

    stages{
      stage("Git Checkout"){
         steps{
            git branch: 'main', changelog: false, poll: false, url: 'https://github.com/balu1123/demo-counter-app.git'
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

      stage("Sonar Analysis"){
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

      stage("OWASP Dependency check"){
        steps{
          script{
            dependencyCheck additionalArguments: '', odcInstallation: 'DP'
            dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
          }
        }
      }
       
      stage("nexus Repo"){
        steps{
          script{
             def pom = readMavenpom file: 'pom.xml'
			       def nexusRepo = pom.version.endwith("SNAPSHOT") ? "demoapp-SNAPSHOT" : "demoapp-release"
			
		    nexusArtifactUploader artifacts: [
                  [artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']
                  ], 
                  credentialsId: 'nexus_cred', 
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
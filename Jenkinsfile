properties([pipelineTriggers([githubPush()])]) 
pipeline {
    agent any
    stages {
        stage('Testing') {
        agent { label "agent" } // Define which agent you want to run the pipeline
            steps {
              // Test menggunakan Sonarqube
                script { 
                echo "Begin Testing Using Sonarqube"
                def scannerHome = tool 'sonarqube' ; //sonarqube by Global Tools Configuration
                withSonarQubeEnv('sonarqube') {  //sonarqube by Endpoint Server Sonarqube
                sh "${scannerHome}/bin/sonar-scanner"}
                } 
              }
            }
        stage('Build') {
        agent { label "agent" } // Define which agent you want to run the pipeline
            steps {
              // Build Image
                script { 

                echo "Begin Build"
                
                if (env.BRANCH_NAME == "stagging")
                
                { 
                sh "docker build -t harjay88/cilistfe-stg:$BUILD_NUMBER frontend/. "
                sh "docker build -t harjay88/cilistbe-stg:$BUILD_NUMBER backend/. "
                sh "docker push harjay88/cilistfe-stg:$BUILD_NUMBER"
                sh "docker push harjay88/cilistbe-stg:$BUILD_NUMBER"
                }
                else
                { 
                sh "docker build -t harjay88/cilistfe-prod:$BUILD_NUMBER frontend/. "
                sh "docker build -t harjay88/cilistbe-prod:$BUILD_NUMBER backend/. "
                sh "docker push harjay88/cilistfe-prod:$BUILD_NUMBER"
                sh "docker push harjay88/cilistbe-prod:$BUILD_NUMBER"
                }
              }
            }
        stage('Deploy') {
        agent { label "agent" }  // Define which agent you want to run the pipeline
            steps {
              // Deploy to Kubernetes
                script { 
                
                echo "Begin to Deploy" 

                if (env.BRANCH_NAME == "stagging")
                {
                sh "kubectl set image deployment/cilistfe cilistfe=harjay88/cilistfe-stg:$BUILD_NUMBER -n stagging"
                sh "kubectl set image deployment/cilistbe cilistbe=harjay88/cilistbe-stg:$BUILD_NUMBER -n stagging"
                sh "docker image rmi harjay88/cilistfe-stg:$BUILD_NUMBER"
                sh "docker image rmi harjay88/cilistbe-stg:$BUILD_NUMBER"
                }
                else
                {
                sh "kubectl set image deployment/cilistfeprod cilistfeprod=harjay88/cilistfe-prod:$BUILD_NUMBER -n production"
                sh "kubectl set image deployment/cilistbeprod cilistbeprod=harjay88/cilistbe-prod:$BUILD_NUMBER -n production"
                sh "docker image rmi harjay88/cilistfe-prod:$BUILD_NUMBER"
                sh "docker image rmi harjay88/cilistbe-prod:$BUILD_NUMBER"
                }
              }
            }
          }
        }
      }
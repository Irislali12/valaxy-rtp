def imageName = 'iris-demo/valaxy-rtp'
def registry  = '883961463906.dkr.ecr.us-east-1.amazonaws.com/iris-demo'
def version   = '1.0.2'
def app
pipeline {
    agent {
       node {
         label "liam"
      }
    }
    stages {
        stage('Build') {
            steps {
                echo '<--------------- Building --------------->'
                sh 'printenv'
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo '<------------- Build completed --------------->'
            }
        }
        stage('Unit Test') {
            steps {
                echo '<--------------- Unit Testing started  --------------->'
                sh 'mvn surefire-report:report'
                echo '<------------- Unit Testing stopped  --------------->'
            }
        }
        
        stage ("Sonar Analysis") {
            environment {
               scannerHome = tool 'SonarQubeScanner'
            }
            steps {
                echo '<--------------- Sonar Analysis started  --------------->'
                withSonarQubeEnv('SonarQube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }    
                echo '<--------------- Sonar Analysis stopped  --------------->'
            }   
        }    
        
        
        stage(" Docker Build ") {
          steps {
            script {
               echo '<--------------- Docker Build Started --------------->'
               app = docker.build(imageName+":"+version)
               echo '<--------------- Docker Build Ends --------------->'
            }
          }
        }
        
        
        // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 883961463906.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker push 883961463906.dkr.ecr.us-east-1.amazonaws.com/iris-demo:1.0.2'
         }
        }
      }

        stage(" Deploy ") {
          steps {
            script {
               echo '<--------------- Deploy Started --------------->'
               sh './deploy.sh'
               echo '<--------------- Deploy Ends --------------->'
            }
          }
        }
          
    }
 }

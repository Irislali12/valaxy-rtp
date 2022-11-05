def imageName = 'stalinrtp.jfrog.io/valaxy-docker/valaxy-rtp'
def registry  = 'https://stalinrtp.jfrog.io'
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
    }
 }

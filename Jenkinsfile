
pipeline {
  agent any;
  environment {
     SSH = credentials("rolex-ssh-user")
  }


  stages {
    stage("develop server") {
        agent {
          docker {
            image 'node:8-alpine'
            args '-p 3000:3000'
          }
        }

        stages {
          stage("Install dependencies"){
            steps{
              sh 'yarn install'
            }
          }

          stage('run test') {
            steps {
              sh 'echo "running test" && ls -lah'
              sh './jenkins/scripts/test.sh'
            }
          }

          stage('serve') {
            steps {
              sh 'ls -lah'
              sh './jenkins/scripts/deliver.sh'
              input message: 'Finished using the web site?'
              sh './jenkins/scripts/kill.sh'
            }
          }
        }
    }

    stage("staging server") {
        agent {
          docker {
            image 'node:8-alpine'
            args '-p 3000:3000'
          }
        }

        stages {
          stage("Install dependencies"){
            steps{
              sh 'yarn install'
            }
          }

          stage('run test') {
            steps {
              sh 'echo "running test" && ls -lah'
              sh './jenkins/scripts/test.sh'
            }
          }

          stage('serve') {
            steps {
              sh 'ls -lah'
              sh './jenkins/scripts/deliver.sh'
              input message: 'Finished using the web site?'
              sh './jenkins/scripts/kill.sh'
            }
          }
        }
    }
  }
}

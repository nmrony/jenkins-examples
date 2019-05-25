
pipeline {

  agent {
    docker {
      image 'node:9-alpine'
      args '-p 3000:3000'
    }
  }

  environment {
     SSH = credentials("rolex-ssh-user")
  }

  stages {
    stage("Install dependencies"){
      steps{
        sh 'npm install'
      }
    }

    stage('run test') {
      steps {
        sh './jenkins/scripts/test.sh'
      }
    }

    stage('serve') {
      steps{
         sh './jenkins/scripts/deliver.sh'
         input message: 'Finished using the web site? (Click "Proceed" to continue)'
         sh './jenkins/scripts/kill.sh'
      }
    }

    stage('deploy') {
       input {
          message "Should we continue?"
          ok "Yes, we should."

      }
      stages {
        stage('production build') {
          steps {
            sh 'rm -fr ./build && npm run build'
          }
        }
        stage('Publish') {
          steps {
            sh 'ssh $SSH_USR:$SSH_PSW@192.168.0.1 rm -rf ~/projects/test'
            sh 'ls -lah'
          }
        }
      }
    }
  }
}

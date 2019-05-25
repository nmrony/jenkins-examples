
pipeline {

  agent {
    docker {
      image 'node:9-alpine'
      args '-p 3000:3000'
    }
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
        message: 'Deploy to server?'
      }
      stages {
        stage('production build') {
          steps {
            sh 'rm -fr ./build && npm run build'
          }
        }
        stage('Publish') {
          def remote = [:]
          remote.name = "playground-vbox"
          remote.host = "192.168.0.105"
          remote.allowAnyHosts = true
          remote.fileTransfer = 'scp'
          withCredentials([
            usernamePassword(
              credentialsId: 'rolex-ssh-user',
              passwordVariable: 'password',
              usernameVariable: 'username')
          ]) {
            sshPut remote: remote, from: './build/*', into: '~/projects/test'
          }
        }
      }
    }
  }
}

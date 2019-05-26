
pipeline {

  agent {
    docker {
      image 'node:8-alpine'
      args '-p 3000:3000'
    }
  }
  environment {
     SSH = credentials("rolex-ssh-user")
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

    // stage('deploy') {
    //    input {
    //       message "Should we continue?"
    //       ok "Yes, we should."

    //   }
    //   stages {
    //     stage('production build') {
    //       steps {
    //         sh 'rm -fr ./build && npm run build'
    //       }
    //     }
    //     stage('Publish') {
    //       steps {
    //         sh 'ssh $SSH_USR:$SSH_PSW@192.168.0.105 rm -rf ~/projects/test'
    //         sh 'ls -lah'
    //       }
    //     }
    //   }
    // }
  }
}

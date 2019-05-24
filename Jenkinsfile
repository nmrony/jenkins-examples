pipeline {
    agent none;
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:9-alpine'
                    args '-p 3000:3000'
                }
            }
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:9-alpine'
                    args '-p 3000:3000'
                }
            }
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Check') {
            agent {
                docker {
                    image 'node:9-alpine'
                    args '-p 3000:3000'
                }
            }
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }

        stage('Deploy') {
            steps {
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
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

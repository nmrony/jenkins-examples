#!/usr/bin/env groovy

node {
  def app
  pipeline {
    stages {
      stage('Clone') {
        steps {
          // Let's make sure we have the repository cloned to our workspace
          checkout scm
        }
      }

      stage('Build image') {
        steps {
          // This builds the actual image; synonymous to docker build on the command line
          app = docker.build("nmrony/jenkins-node")
        }
      }

      stage('Build') {
        steps {
          app.inside() {
            sh 'yarn install'
          }
        }
      }

      stage('Test image') {
        steps {
          // Ideally, we would run a test framework against our image.
          app.inside() {
              sh 'echo "Tests passed"'
          }
        }
      }

      stage('Deliver') {
        steps {
          app.inside("-p 3000:8000") {
            sh './scripts/deliver.sh'
            input message: 'Finished using the web site? (Click "Proceed" to continue)'
            sh './scripts/kill.sh'
          }
        }
      }

      // stage('Push image') {
      //     // Finally, we'll push the image with two tags:
      //      * First, the incremental build number from Jenkins
      //      * Second, the 'latest' tag.
      //      * Pushing multiple tags is cheap, as all the layers are reused. */
      //     docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
      //         app.push("${env.BUILD_NUMBER}")
      //         app.push("latest")
      //     }
      // }
    }
  }

}
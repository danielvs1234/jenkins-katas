pipeline {
  agent any
  stages {
    stage('Parallel exection') {
      parallel {
        stage('Say hello!') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('Build App') {
          agent {
            docker {
              image 'gradle:jdk11'
            }

          }
          steps {
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
            sh 'echo "THIS IS WHERE I AM"'
            sh 'ls'
            deleteDir()
            sh 'ls'
          }
        }

      }
    }

  }
}
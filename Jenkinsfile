pipeline {
    environment{
    docker_username = 'd0wnt0wn3d'
    }
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
            stash 'mycode'
           
          }
        }

      }
    }
    stage('push docker app'){
        environment {
          DOCKERCREDS = credentials('docker_login')
        }
        steps {
            unstash 'mycode' //unstash the repository code
            sh 'ci/build-docker.sh'
            sh 'echo "$DOCKERCREDS_PSW" | docker login -u "$DOCKERCREDS_USR" --password-stdin' //login to docker hub with the credentials above
            sh 'ci/push-docker.sh'
        }
    }

  }
  post{
      always {
           sh 'ls'
           deleteDir()
            sh 'ls'
      }
  }
}
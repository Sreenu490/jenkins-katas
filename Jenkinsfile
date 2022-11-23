pipeline {
  agent any
  stages {
    stage('paralle execution') {
      parallel {
        stage('Hello') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('Bulid app') {
          agent {
            docker {
              image 'gradle:6-jdk11'
            }

          }
          steps {
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
            sh 'ls -la'
            deleteDir()
            sh 'ls -la'
          }
        }

      }
    }

  }
}

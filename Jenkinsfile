pipeline {
  agent any
  stages {
    stage('clone down') {
      agent {
      label 'master-label'
      }
          steps {
           stash excludes: '.git' , name: 'code' 
          }
        }
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
          options {
          skipDefaultCheckout true
          }
          steps {
            unstash 'code'
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

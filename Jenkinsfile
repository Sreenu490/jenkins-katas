pipeline {
  agent any
  environment { 
        docker_username = 'sreenu321'
    }
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
            stash 'code'
            archiveArtifacts 'app/build/libs/'
            sh 'ls -la'
            deleteDir()
            sh 'ls -la'
          }
        }
        
        stage('test app') {
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
            sh 'ci/unit-test-app.sh'
            junit 'app/build/test-results/test/TEST-*.xml'
            
          }
        }
        
        
        

      }
    }
stage('push docker app') {
    environment {
      DOCKERCREDS = credentials('docker_login') //use the credentials just created in this stage
}
steps {
      unstash 'code' //unstash the repository code
      sh 'ci/build-docker.sh'
      sh 'echo "$DOCKERCREDS_PSW" | docker login -u "$DOCKERCREDS_USR" --password-stdin' //login to docker hub with the credentials above
      sh 'ci/push-docker.sh'
}
        }
  }
  post {
    cleanup {
        deleteDir() /* clean up our workspace */
    }
}
  
}

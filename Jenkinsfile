pipeline {
  agent any
  stages {
    stage('clone down') {
      agent {
        node {
          label 'master-label'
        }

      }
      steps {
        stash(name: 'code', excludes: '.git')
      }
    }

    stage('Say Hello') {
      parallel {
        stage('Say Hello') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('build app') {
          agent {
            docker {
              image 'gradle:jdk11'
            }

          }
          options {
            skipDefaultCheckout(true)
          }
          steps {
<<<<<<< HEAD
            stash(name: 'code', includes: '.git')
            sh 'skipDefaultCheckout(true)'
=======
            unstash 'code'
            skipDefaultCheckout true
>>>>>>> e60dc3ce489cf54fc4ffec4f0dccb212abd14adc
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
            sh '''ls
deleteDir()
ls'''
          }
        }

        stage('test app') {
          agent {
            docker {
              image 'gradle:jdk11'
            }

          }
          steps {
<<<<<<< HEAD
            stash(name: 'code', includes: '.git')
=======
            unstash 'code'
>>>>>>> e60dc3ce489cf54fc4ffec4f0dccb212abd14adc
          }
        }

      }
    }

  }
}
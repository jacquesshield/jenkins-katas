pipeline {
  agent any
  stages {
    stage('clone down') {
      agent {
        node {
          label 'master/host'
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
          steps {
            stash(name: 'code', includes: '.git')
            sh 'skipDefaultCheckout(true)'
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
            stash(name: 'code', includes: '.git')
          }
        }

      }
    }

  }
}
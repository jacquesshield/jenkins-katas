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
            unstash 'code'
            skipDefaultCheckout true
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
            unstash 'code'
            sh 'ci/unit-test-app.sh'
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
  environment {
    docker_username = 'defetab134btsese'
  }
}
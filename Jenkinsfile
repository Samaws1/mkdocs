pipeline {
  environment {
    registry = "3.138.33.207:8086"
    registryCredential = 'jenk-nex-dkr'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/Samaws1/mkdocs'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Test Mkdocs' ) {
                agent {
                docker { image 'anishnath/mkdocs:$BUILD_NUMBER' }
            }
            steps {
                sh 'mkdocs --version'
            }
        }


    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry/jenktest:$BUILD_NUMBER"
      }
    }
  }
}

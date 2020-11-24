pipeline {
  environment {
    registry = "anishnath/mkdocs"
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
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}

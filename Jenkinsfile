pipeline {

  environment {
    registry = "cloudbabu"
    dockerImage = ""
    DOCKERHUB_CREDENTIALS=credentials('dockerhub')
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/babubalagani/training.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "", DOCKERHUB_CREDENTIALS ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}

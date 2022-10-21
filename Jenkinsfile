pipeline{
  agent any
  environment {
    imagename = "khc12140185/docker-jenkins"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  stages{
    stage('Cloning Git repo'){
      steps {
        git([url: 'https://github.com/18mis7023/docker-demo.git', branch: 'master'])
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"
      }
    }
  }
}
pipeline {
  agent any
  stages {
    stage('Docker build') {
      steps {
        sh 'docker build -t webapp:$BUILD_NUMBER .'
      }
    }
    stage('Docker push to development') {
          when {
              branch 'Development'
          }
      steps {
        withCredentials(bindings: [usernamePassword(credentialsId: 'acr', passwordVariable: 'pass', usernameVariable: 'user')]) {
          sh '''echo "$pass" | docker login ${registry} --username $user --password-stdin
docker tag webapp:$BUILD_NUMBER ${registry}:dev
docker push ${registry}:dev'''
        }

      }
    }
    stage('Docker push to master') {
          when {
              branch 'master'
          }
      steps {
        withCredentials(bindings: [usernamePassword(credentialsId: 'acr', passwordVariable: 'pass', usernameVariable: 'user')]) {
          sh '''echo "$pass" | docker login ${registry} --username $user --password-stdin
docker tag webapp:$BUILD_NUMBER ${registry}
docker push ${registry}'''
        }

      }
    }
  }
  environment {
    registry = 'demo35264758697.azurecr.io/webapp'
  }
}

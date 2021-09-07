pipeline {
  environment {
    registryCredential = "17hema"
  }
  agent any
  stages {
    stage('code complile') {
      steps{
        script {
          sh 'mvn clean install'
        }
      }
    }
    stage('build image') {
      steps{
        script {
          app = docker.build("17hema/simple-spring:${env.BUILD_ID}")
        }
      }
    }
     stage('push image') {
      steps{
        script {
          docker.withRegistry( "https://registry.hub.docker.com", registryCredential ) {
           // dockerImage.push()
          app.push("latest")
          app.push("${env.BUILD_ID}")
          }
        }
      }
    }
    stage('Deploy to GKE test cluster') {
            steps{
                sh "sed -i 's/17hema/simple-spring:${env.BUILD_ID}/g' sample.yaml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME_TEST, location: env.LOCATION, manifestPattern: 'sample.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }
  }
}

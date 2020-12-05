pipeline {
 
 agent any
 
 environment {
    PROJECT_ID = "original-brace-289402"
    CLUSTER_NAME = 'cluster-1'
    LOCATION = 'us-central1-c'
    CREDENTIALS_ID = 'MyProject'
  }   
 stages {
     stage('Checkout SCM') {
      steps {
       echo "Pull del codigo"
       checkout scm
      }
     }
    stage('Build package') {

        steps {
        echo "Construcción del paquete"
         sh 'mvn clean package'
        }
    }
     stage('Test') {
      steps {
       echo "Testing"
       sh 'mvn test'
      }
     }
 stage('Build and push Docker Image') {
      steps{
        script {
           appimage = docker.build("eudreyagudelo/devops:${env.BUILD_ID}")
           docker.withRegistry('https://registry.hub.docker.com', 'dockerhub')   {
           appimage.push("${env.BUILD_ID}")
           }     
         }
       }
      }
    
     stage('Deploy to Kubernetes') {
      steps {
       sh 'ls -ltr'
       sh 'pwd'
       sh "sed -i 's/tagversion/${env.BUILD_ID}/g' deployment.yaml"
       step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
       echo "Deploy practica final kubernetes cluster"
      }
     }
}
}

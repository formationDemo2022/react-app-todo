pipeline {

  environment {
    dockerimagename = "mohamed1780/app-react-todos"
    dockerImage = ""
    DOCKER_BINARY = "/usr/bin/docker"
  }

  agent any 
  
  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/formationDemo2022/react-app-todo.git'
      }
    }

    stage('Build image') {
      agent any 
      steps{
        script {
          dockerImage = ${DOCKER_BINARY} build -t dockerimagename .
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'credential-docker'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }

}
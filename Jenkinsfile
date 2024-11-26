pipeline {
    
    environment {
    dockerimagename = "rahat6/rahatk8s-nodeapp:latest"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/rahatdevops/nodeapp_test.git'
      }
    }
    stage("Docker build"){
        steps {
        sh 'docker version'
        sh 'docker build -t rahatk8s-nodeapp .'
        sh 'docker image list'
        sh 'docker tag rahatk8s-nodeapp rahat6/rahatk8s-nodeapp:latest'
     } 
    }
    
    stage("Login Docker Hub") {
            steps {
    withCredentials([string(credentialsId: 'dockerhublogin', variable: 'PASSWORD')]) {
        sh 'docker login -u rahat6 -p $PASSWORD'
        sh 'docker push rahat6/rahatk8s-nodeapp:latest'
        }
        
      }
    }
    

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy (configs: 'deploymentservice.yml', kubeConfig: [path: ''], kubeconfigId: 'kubernetes')
        }
      }
    }

  }

}

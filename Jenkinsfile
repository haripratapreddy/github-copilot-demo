pipeline {
  agent any

  tools {
    maven 'mvn'
  }

  stages {
    stage('Build App') {
      steps {
        sh 'mvn package'
      }
    }
    
    stage('Build Image') {
      steps {
      sh "docker build -t harip220/github-copilot-demo:${env.BUILD_ID} ."
      sh "docker tag harip220/github-copilot-demo:${env.BUILD_ID} harip220/github-copilot-demo:latest"
      }
    }
    
  }

  post {
    always {
      archive 'target/**/*.jar'
    }
    success {
      withCredentials([usernamePassword(credentialsId: 'docker_hub_credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh "docker login -u ${USERNAME} -p ${PASSWORD}"
        sh "docker push harip220/github-copilot-demo:${env.BUILD_ID}"
        sh "docker push harip220/github-copilot-demo:latest"
      }
    }
  }
}

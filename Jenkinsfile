pipeline {
  agent any

  environment {
    DOCKER_REGISTRY = 'https://harbor.infra.longitood.com'
    REPOSITORY_URL = 'https://github.com/longitood/TypeSwift'
    PROD_PROJECT_UUID = 'hcw0g4sog0o8k8k0gokc4c0o'
    NODE_BASE_IMAGE = 'node:20-alpine'
    PROJECT_NAME = 'typeswift/typeswift'
  }

  stages {
    stage('Checkout') {
      steps {
        git url: "$REPOSITORY_URL", branch: env.BRANCH_NAME
      }
    }

    stage('Build docker image and publish to registry') {
      when {
        anyOf {
          branch "main"
        }
      }
      steps {
        script {
            docker.withRegistry("${DOCKER_REGISTRY}", 'harbor-registry-credentials') {
              def customImage = docker.build("${env.PROJECT_NAME}:${env.BRANCH_NAME.replace("/","-")}")
              customImage.push()
            }
        }
      }
    }
  }

  post {
    success {
      echo 'Build successful!'
    }
    failure {
      echo 'Build failed!'
    }
  }
}

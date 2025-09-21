pipeline {
  agent any
  parameters {
    string(name: 'BRANCH_NAME', defaultValue: 'dev', description: 'Branch to build')
    string(name: 'DOCKER_TAG_SUFFIX', defaultValue: '', description: 'Optional Docker tag suffix')
  }
  environment {
    BRANCH_SANITIZED = "${params.BRANCH_NAME.replace('/', '-').replace('_', '-')}"
    DOCKER_IMAGE_TAG = "${BRANCH_SANITIZED}-${env.BUILD_NUMBER}${params.DOCKER_TAG_SUFFIX}"
  }
  stages {
    stage('Checkout') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: "${params.BRANCH_NAME}"]],
                  userRemoteConfigs: [[url: 'https://github.com/Abhaytomar2004/CI-CD-Youtube-Clone.git']]])
      }
    }
    stage('Install Backend Dependencies') {
      steps {
        dir('backend') {
          bat 'npm install'
        }
      }
    }
    stage('Install Frontend Dependencies') {
      steps {
        dir('frontend') {
          bat 'npm install'
        }
      }
    }
    stage('Build Frontend') {
      steps {
        dir('frontend') {
          bat 'npm run build'
        }
      }
    }
    stage('Docker Compose Build & Up') {
      steps {
        bat 'docker-compose build'
        bat 'docker-compose up -d'
      }
    }
    stage('Docker Compose Down') {
      steps {
        bat 'docker-compose down'
      }
    }
  }
  post {
    success {
      echo "Build and deployment succeeded for branch: ${params.BRANCH_NAME}"
    }
    failure {
      echo "Build or deployment failed for branch: ${params.BRANCH_NAME}"
    }
    always {
      cleanWs()
    }
  }
}

pipeline {
  agent any

  tools {
    jdk 'JDK17'
    maven 'Maven-3.9'
  }

  options {
    timestamps()
    disableConcurrentBuilds()
  }

  environment {
    MVN = "mvn -B -U"
  }

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh "${MVN} clean compile"
      }
    }

    stage('Unit Test') {
      steps {
        sh "${MVN} test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }

    stage('Package') {
      steps {
        sh "${MVN} package"
      }
      post {
        success {
          archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
      }
    }
  }

  post {
    success { echo "✅ Java pipeline completed successfully" }
    failure { echo "❌ Java pipeline failed - check console logs" }
  }
}


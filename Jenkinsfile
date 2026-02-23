pipeline {
  agent {
    docker {
      image 'maven:3.9.9-eclipse-temurin-17'
      args '-v $HOME/.m2:/root/.m2'
    }
  }

  triggers {
    cron('H/5 * * * 1')   // Mondays every 5 minutes
  }

  options { timestamps() }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build & Test (JaCoCo)') {
      steps {
        sh 'mvn -B clean test jacoco:report'
      }
      post {
        always {
          junit allowEmptyResults: true, testResults: '**/target/surefire-reports/*.xml'
          archiveArtifacts artifacts: '**/target/*.jar, **/target/site/jacoco/**', allowEmptyArchive: true
        }
      }
    }
  }
}

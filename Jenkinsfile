pipeline {
  agent any

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
          junit '**/target/surefire-reports/*.xml'
          archiveArtifacts artifacts: '**/target/*.jar, **/target/site/jacoco/**', allowEmptyArchive: true
        }
      }
    }
  }
}

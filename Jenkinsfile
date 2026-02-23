pipeline {
  agent any

  triggers {
    cron('H/5 * * * 1')
  }

  options { timestamps() }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build & Test (JaCoCo)') {
      steps {
        sh '''
          chmod +x mvnw
          ./mvnw -B clean test jacoco:report
        '''
      }
    }
  }

  post {
    always {
      junit allowEmptyResults: true, testResults: '**/target/surefire-reports/*.xml'

      publishHTML(target: [
        allowMissing: false,
        alwaysLinkToLastBuild: true,
        keepAll: true,
        reportDir: 'target/site/jacoco',
        reportFiles: 'index.html',
        reportName: 'JaCoCo Report'
      ])

      archiveArtifacts artifacts: '**/target/*.jar, **/target/site/jacoco/**', allowEmptyArchive: true
    }
  }
}

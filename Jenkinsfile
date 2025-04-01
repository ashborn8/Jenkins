pipeline {
  agent any

  tools {
    nodejs 'NodeJS22'
  }

  environment {
    SONARQUBE_SERVER = 'sonarqube'
    SONAR_SCANNER_HOME = '/var/jenkins_home/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarScanner'
  }

  stages {
    stage('Instalar dependencias') {
      steps {
        sh 'npm install'
      }
    }

    stage('Análisis SonarQube') {
      steps {
        withSonarQubeEnv("${SONARQUBE_SERVER}") {
          withCredentials([string(credentialsId: 'sonarqube_auth_token', variable: 'SONAR_TOKEN')]) {
            sh """
              \${SONAR_SCANNER_HOME}/bin/sonar-scanner \\
                -Dsonar.projectKey=asignacion-frontend \\
                -Dsonar.sources=src \\
                -Dsonar.host.url=http://sonarqube:9000 \\
                -Dsonar.token=\${SONAR_TOKEN} \\
                -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info \\
                -Dsonar.eslint.reportPaths=eslint-report.json
            """
          }
        }
      }
    }
  }
}

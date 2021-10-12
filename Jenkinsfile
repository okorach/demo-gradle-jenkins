
pipeline {
  agent any
  stages {
    stage('SonarQube LATEST analysis') {
      steps {
        withSonarQubeEnv('SQ Latest') {
          script {
            sh 'cd comp-gradle; ./gradlew jacocoTestReport sonarqube'
          }
        }
      }
    }
    stage("SonarQube LATEST Quality Gate") {
      steps {
        timeout(time: 5, unit: 'MINUTES') {
          script {
            def qg = waitForQualityGate()
            if (qg.status != 'OK') {
              echo "Gradle component quality gate failed: ${qg.status}, proceeding anyway"
            }
            sh 'rm -f comp-gradle/build/sonar/report-task.txt'
          }
        }
      }
    }
  }
}
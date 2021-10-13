pipeline {
  agent any
  stages {
    stage('Code Checkout') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: '**']], extensions: [[$class: 'CloneOption', noTags: false, reference: '', shallow: false]], userRemoteConfigs: [[credentialsId: 'github-app', url: 'https://github.com/okorach/demo-gradle-jenkins']]])
      }
    }
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
          }
        }
      }
    }
  }
}
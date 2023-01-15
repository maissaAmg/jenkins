pipeline {
  agent any
  stages {
    stage("test") {
          steps {
            junit(testResults: 'build/reports/tests/test', allowEmptyResults: true)
            archiveArtifacts 'build/reports/tests/test/*'
            cucumber 'target/report.json'
            
          }
        }
    
    stage('Code Analysis') {
          steps {
            withSonarQubeEnv('jenkins') {
              bat 'sonar-scanner'
            }
          }
        }
    
    stage('Code Quality') {
          steps {
           waitForQualityGate abortPipeline: true
          }
        }
      
    stage('build') {
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        archiveArtifacts 'build/docs/javadoc/*'

      }
    }

    stage('Deploy') {
      steps {
        bat 'gradle publish'
      }
    }
    
    stage("Notification"){
      steps{
        post {
        success {
          mail(subject: 'Pipeline Success', body: 'Pipeline déployé avec succès !', from: 'jm_amghar@esi.dz', to: 'jm_amghar@esi.dz')
        }
        failure {
          mail(subject: 'Pipeline Failure', body: "Pipeline n'a pas été déployé avec succès !", from: 'jm_amghar@esi.dz', to: 'jm_amghar@esi.dz')
        }
           }
        notifyEvents message: 'Pipeline terminée', token: '01234567890123456789012345678901'
        }
    }
}

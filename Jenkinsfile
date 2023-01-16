pipeline {
  agent any
  stages {
    stage("test") {
          steps {
            bat 'gradle test'
            junit(testResults: 'build/reports/tests/test', allowEmptyResults: true)
            cucumber reportTitle: 'cucumber report',fileIncludePattern:'target/report.json'
            
          }
        }
    
    stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              bat 'gradle sonar'
            }
          }
        }
    
    stage('Code Quality') {
          steps {
           timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
             }
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
        post {
                success {
                  mail(subject: 'Pipeline Success', body: 'Pipeline deploye avec succes !', from: 'jm_amghar@esi.dz', to: 'jm_amghar@esi.dz')
                }
                failure {
                  mail(subject: 'Pipeline Failure', body: "Pipeline n'a pas ete deploye avec succes !", from: 'jm_amghar@esi.dz', to: 'jm_amghar@esi.dz')
                }
                   }
      steps{
        notifyEvents message: 'Pipeline terminee', token: '_Llsr3gFylnymvmkH3zyUVKSXC3oTijH'
        }
    }

}

}


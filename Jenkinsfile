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
      parallel {
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('TP8_OGL_JENKINS') {
              bat 'sonar-scanner'
            }

          }
        }
      }
    }
    
    stage('build') {
      post {
        success {
          mail(subject: 'Build Success', body: 'New Build is deployed !', from: 'jm_amghar@esi.dz', to: 'jm_amghar@esi.dz')
        }
        failure {
          mail(subject: 'Build Failure', body: "the new build isn't deployed succesfully !", from: 'jm_amghar@esi.dz', to: 'jm_amghar@esi.dz')
        }
        
      }
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

    stage('Slack Notifications') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', token: 'T02S71TC7EH/B02SZN07R08/df3SCOkFu6oGC9VYtrwEVUD6', message: 'New build is Created', channel: 'OGL')
      }
    }

  }
}

pipeline {
  options {
    disableConcurrentBuilds()
  }
  agent {
    label "jenkins-maven"
  }
  environment {
    DEPLOY_NAMESPACE = "production"
    GIT_MESSAGE = sh (script:'git log --oneline -1 ${GIT_COMMIT}', returnStdout: true)
  }
  stages {
    stage('Validate Environment') {
      steps {
        container('maven') {
          dir('env') {
            sh 'jx step helm build'
          }
        }
      }
    }
    stage('Update Environment') {
      when {
        branch 'master'
      }
      steps {
        container('maven') {
          dir('env') {
            sh 'jx step helm apply'
          }
        }
      }
    }
  }
  post {
    success {
          rocketSend attachments: [
            [
              audioUrl: '',
              authorIcon: '',
              authorName: '',
              color: 'green',
              imageUrl: '',
              messageLink: '',
              text: 'Success',
              thumbUrl: '',
              title: "Changes: ${env.GIT_MESSAGE}",
              titleLink: '',
              titleLinkDownload: '',
              videoUrl: ''
            ]
          ],
          channel: 'production',
          message: "Deploy into Production #${env.BUILD_NUMBER} finished - (<${env.BUILD_URL}|Open>)",
          rawMessage: true
        }
        failure {
          rocketSend attachments: [
            [
              audioUrl: '',
              authorIcon: '',
              authorName: '',
              color: 'red',
              imageUrl: '',
              messageLink: '',
              text: 'Failure',
              thumbUrl: '',
              title: "Changes: ${env.GIT_MESSAGE}",
              titleLink: '',
              titleLinkDownload: '',
              videoUrl: ''
            ]
          ],
          channel: 'production',
          message: "Deploy into Production #${env.BUILD_NUMBER} finished - (<${env.BUILD_URL}|Open>)",
          rawMessage: true
        }
}

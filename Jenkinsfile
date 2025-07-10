pipeline {
  agent any

  parameters {
    choice(name: 'DEPLOY_ENV', choices: ['dev', 'staging', 'prod'], description: 'Select deployment environment')
  }

  environment {
    MVN_CMD = "mvn"
    SONARQUBE_SERVER = 'mysonar'
    SONAR_PROJECT_KEY = 'mini-jenkins-project'
    SLACK_CHANNEL = '#all-slack-demo'
  }

  options {
    timeout(time: 30, unit: 'MINUTES')
  }

  triggers {
    pollSCM('H/5 * * * *')
  }

  stages {

    stage('Checkout') {
      steps {
        git url: 'https://github.com/AfzalMansuri23/mini-jenkins-project.git', branch: 'main'
      }
    }

    stage('Build & Test') {
      steps {
        sh "${MVN_CMD} clean package"
      }
    }

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv("${SONARQUBE_SERVER}") {
          sh "${MVN_CMD} sonar:sonar -Dsonar.projectKey=${SONAR_PROJECT_KEY}"
        }
      }
    }

    stage('Quality Gate') {
      steps {
        timeout(time: 2, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
      }
    }

    stage('Deploy') {
      when {
        expression { return params.DEPLOY_ENV in ['dev', 'staging', 'prod'] }
      }
      steps {
        script {
          def deployScript = ""

          switch (params.DEPLOY_ENV) {
            case 'dev':
              deployScript = 'deploy-dev.sh'
              break
            case 'staging':
              deployScript = 'deploy-staging.sh'
              break
            case 'prod':
              deployScript = 'deploy-prod.sh'
              break
          }

          sh "chmod +x ${deployScript}"
          sh "./${deployScript}"
        }
      }
    }

    stage('Smoke Test') {
      steps {
        sh "curl --fail http://${params.DEPLOY_ENV}.example.com/health || exit 1"
      }
    }
  }

  post {
    success {
      slackSend(
        channel: "${SLACK_CHANNEL}",
        color: 'good',
        message: "*SUCCESS*\nProject: `${env.JOB_NAME}`\nBuild: #${env.BUILD_NUMBER}\nEnvironment: *${params.DEPLOY_ENV}*"
      )
      archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
    }

    failure {
      slackSend(
        channel: "${SLACK_CHANNEL}",
        color: 'danger',
        message: "*FAILURE*\nProject: `${env.JOB_NAME}`\nBuild: #${env.BUILD_NUMBER}\nEnvironment: *${params.DEPLOY_ENV}*"
      )
    }

    always {
      cleanWs()
    }
  }
}

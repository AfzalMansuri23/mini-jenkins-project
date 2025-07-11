pipeline {
  agent any
  tools{
    maven 'mymaven'
  }

  parameters {
    choice(name: 'DEPLOY_ENV', choices: ['dev', 'staging', 'prod'], description: 'Select deployment environment')
  }

  environment {
  SONAR_TOKEN = credentials('jenkinskey1')
  SONARQUBE_SERVER = 'mysonar'
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
        sh 'mvn clean package'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv("${SONARQUBE_SERVER}") {
        sh """
          mvn sonar:sonar \
          -Dsonar.projectKey=mini-jenkins-project \
          -Dsonar.organization=afzalmansuri233 \
          -Dsonar.host.url=https://sonarcloud.io \
          -Dsonar.login=${SONAR_TOKEN}
         """
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

  }

  post {
    success {
      slackSend(
        channel: "${SLACK_CHANNEL}",
        color: 'good',
        message: ":rocket: *Build SUCCESSFUL!*\n*Project:* `${env.JOB_NAME}`\n*Build:* #${env.BUILD_NUMBER}\n*Environment:* `${params.DEPLOY_ENV}`\n:checkered_flag: *Status:* Success"
      )
      //archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
    }

    failure {
      slackSend(
        channel: "${SLACK_CHANNEL}",
        color: 'danger',
        message: ":x: *Build FAILED!*\n*Project:* `${env.JOB_NAME}`\n*Build:* #${env.BUILD_NUMBER}\n*Environment:* `${params.DEPLOY_ENV}`\n:warning: *Status:* Failed"
      )
    }

    always {
      cleanWs()
    }
  }
}

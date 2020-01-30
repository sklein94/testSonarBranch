#!groovy

node('master') {

  project = 'github.com/sklein/testSonarBranch'

  stage('Checkout') {
    checkout scm
  }

  stage('SonarQube') {
    def scannerHome = tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    withSonarQubeEnv {
      echo ${env.BRANCH_NAME}
      sh "${scannerHome}/bin/sonar-scanner -Dsonar.branch.name=${env.BRANCH_NAME} -Dsonar.projectKey=testSonarBranch:${env.BRANCH_NAME} -Dsonar.projectName=testSonarBranch:${env.BRANCH_NAME}"
    }
    timeout(time: 2, unit: 'MINUTES') { // Needed when there is no webhook for example
        def qGate = waitForQualityGate()
        if (qGate.status != 'OK') {
            unstable("Pipeline unstable due to SonarQube quality gate failure")
        }
    }
  }

}

#!groovy

@Library('github.com/cloudogu/ces-build-lib@59d3e94')
import com.cloudogu.ces.cesbuildlib.*

node('master') {

  project = 'github.com/sklein/testSonarBranch'

  stage('Checkout') {
    checkout scm
  }

  stage('SonarQube') {
    def scannerHome = tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    withSonarQubeEnv {
      String branch = "${env.BRANCH_NAME}"
      echo branch
      if ("${env.BRANCH_NAME}" == "develop"){
         sh "${scannerHome}/bin/sonar-scanner -Dsonar.branch.name=${env.BRANCH_NAME} -Dsonar.projectKey=testSonarBranch -Dsonar.projectName=testSonarBranch"
      } else if ("${env.BRANCH_NAME}" == "master"){
        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=testSonarBranch -Dsonar.projectName=testSonarBranch"
      }
    }
    timeout(time: 2, unit: 'MINUTES') { // Needed when there is no webhook for example
        def qGate = waitForQualityGate()
        if (qGate.status != 'OK') {
            unstable("Pipeline unstable due to SonarQube quality gate failure")
        }
    }
  }

}

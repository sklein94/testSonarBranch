#!groovy

@Library('github.com/cloudogu/ces-build-lib@59d3e94')
import com.cloudogu.ces.cesbuildlib.*

node('master') {

  project = 'github.com/sklein/testSonarBranch'

  stage('Checkout') {
    checkout scm
  }

  stage('SonarQube') {
        echo "${env.CHANGE_BRANCH}"
        echo "${env.CHANGE_TARGET}"
        echo "${env.CHANGE_ID}"
    def scannerHome = tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    withSonarQubeEnv {
      String branch = "${env.BRANCH_NAME}"
      if (branch == "develop"){
         sh "${scannerHome}/bin/sonar-scanner -Dsonar.branch.name=${env.BRANCH_NAME} -Dsonar.projectKey=testSonarBranch -Dsonar.projectName=testSonarBranch"
      } else if (branch == "master"){
        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=testSonarBranch -Dsonar.projectName=testSonarBranch"
      } else if (${env.CHANGE_BRANCH} != null){
        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=testSonarBranch -Dsonar.pullrequest.base=${env.CHANGE_TARGET} -Dsonar.projectName=testSonarBranch"
      }
      else {
        sh "${scannerHome}/bin/sonar-scanner -Dsonar.branch.name=${env.BRANCH_NAME} -Dsonar.projectKey=testSonarBranch -Dsonar.projectName=testSonarBranch"
      }
    }
  }

}

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
        echo "branch is develop"
         sh "${scannerHome}/bin/sonar-scanner -Dsonar.branch.name=${env.BRANCH_NAME} -Dsonar.projectKey=testSonarBranch -Dsonar.projectName=testSonarBranch"
      } else if (branch == "master"){
        echo "branch is master"
        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=testSonarBranch -Dsonar.projectName=testSonarBranch"
      } else if (env.CHANGE_TARGET){        
        echo "branch is pull request"
        sh "${scannerHome}/bin/sonar-scanner -Dsonar.pullrequest.key=${env.CHANGE_ID} -Dsonar.pullrequest.branch=${env.CHANGE_BRANCH} -Dsonar.projectKey=testSonarBranch -Dsonar.branch.name=${env.CHANGE_BRANCH}-PR${env.CHANGE_ID} -Dsonar.branch.target=${env.CHANGE_TARGET} -Dsonar.pullrequest.base=${env.CHANGE_TARGET} -Dsonar.projectName=testSonarBranch"
      }
      else {
        echo "branch is soemthing else"
        sh "${scannerHome}/bin/sonar-scanner -Dsonar.branch.name=${env.BRANCH_NAME} -Dsonar.projectKey=testSonarBranch -Dsonar.projectName=testSonarBranch"
      }
    }
  }

}

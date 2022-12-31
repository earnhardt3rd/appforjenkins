pipeline {
  agent any
  environment {
    APPSYSID = '167bc2662f0021100c8eae5df699b680'
    BRANCH = "${BRANCH_NAME}"
    CREDENTIALS = '18be2029-2e62-4070-8828-dbb3aa39f0f0'
    DEVENV = 'https://dev65108.service-now.com/'
    DEVCRED = 'snow-outlook'
    TESTENV = 'https://dev105694.service-now.com/'
    TESTCRED = 'snow-mmc'
    PRODENV = 'https://dev77308.service-now.com/'
    PRODCRED = 'smow-try'
    TESTSUITEID = 'b1ae55eedb541410874fccd8139619fb'
  }
  stages {
    stage('Build') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snApplyChanges(appSysId: "${APPSYSID}", branchName: "${BRANCH}", url: "${DEVENV}", credentialsId: "${DEVCRED}")
        snPublishApp(credentialsId: "${DEVCRED}", appSysId: "${APPSYSID}", obtainVersionAutomatically: true, url: "${DEVENV}")
      }
    }
    stage('Test') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snInstallApp(credentialsId: "${TESTCRED}", url: "${TESTENV}", appSysId: "${APPSYSID}")
        snRunTestSuite(credentialsId: "${TESTCRED}", url: "${TESTENV}", testSuiteSysId: "${TESTSUITEID}", withResults: true)
      }
    }
    stage('Deploy to Prod') {
      when {
        branch 'master'
      }
      steps {
        snInstallApp(credentialsId: "${PRODCRED}", url: "${PRODENV}", appSysId: "${APPSYSID}")
      }
    }
  }
}

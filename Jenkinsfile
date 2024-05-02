// Filename: Jenkinsfile
node {
  def GITREPOREMOTE = "https://github.com/Aswinkks/dab-jenkins.git"
  def GITBRANCH     = "main"
  def DBCLIPATH     = "/opt/homebrew/bin"
  def JQPATH        = "/opt/homebrew/bin"
  def JOBPREFIX     = "jenkins-demo"
  def BUNDLETARGET  = "dev"

  stage('Checkout') {
    git branch: GITBRANCH, url: GITREPOREMOTE
  }
  stage('creds') {
    sh """#!/bin/bash
          databricks configure --token <<EOF
          https://centralus.azuredatabricks.net
          dapXXXXXXXXXXXXXXXXXXXXXX467
          EOF
       """
  }
  stage('Validate Bundle') {
    sh """#!/bin/bash
          cd dab_p2
          ${DBCLIPATH}/databricks bundle validate -t ${BUNDLETARGET}
       """
  }
  
  stage('Deploy Bundle') {
    sh """#!/bin/bash
          cd dab_p2
          ${DBCLIPATH}/databricks bundle deploy -t ${BUNDLETARGET}
       """
  }
}

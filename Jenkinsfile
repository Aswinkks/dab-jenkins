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
  stage('Validate Bundle') {
    sh """#!/bin/bash
          adburl="https://centralus.azuredatabricks.net"
          token=dapXXXXXXXXXXXXXXXXXXXXXXXXX467
          databricks configure --token <<EOF
          $adburl
          $token
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

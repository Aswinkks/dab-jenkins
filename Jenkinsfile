// Filename: Jenkinsfile
node {
  def GITREPOREMOTE = "https://github.com/Aswinkks/dab-jenkins.git"
  def GITBRANCH     = "main"
  def DBCLIPATH     = "/opt/homebrew/bin"
  def JQPATH        = "/opt/homebrew/bin"
  def JOBPREFIX     = "jenkins-demo"
  def BUNDLETARGET  = "dev"
  def DBCLI_VERSION = "0.218.0"

  stage('Checkout') {
    git branch: GITBRANCH, url: GITREPOREMOTE
  }
  // stage('creds') {
  //   sh """#!/bin/bash
  //         ${DBCLIPATH}/databricks configure --token <<EOF
  //         https://centralus.azuredatabricks.net
  //         dapXXXXXXXXXXXXXXXXXXXXXX467
  //         EOF
  //      """
  // }

stage('Install Databricks CLI') {
    sh """#!/bin/bash
          brew install databricks
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

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
          dab4
          ${DBCLIPATH}/databricks bundle validate -t ${BUNDLETARGET}
       """
  }
  stage('Deploy Bundle') {
    sh """#!/bin/bash
          cd dab4
          ${DBCLIPATH}/databricks bundle deploy -t ${BUNDLETARGET}
       """
  }
}

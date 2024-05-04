pipeline {
    agent any

    environment {
        GITREPOREMOTE = "https://github.com/Aswinkks/dab-jenkins.git"
        GITBRANCH     = "main"
        DBCLIPATH     = "/usr/local/bin/"
        BUNDLETARGET  = "dev"
    }

    
    stages {
        stage('Checkout') {
            steps {
                script {
                    git branch: "${GITBRANCH}", url: "${GITREPOREMOTE}"
                }
            }
        }

        stage('Databricks Configure') {
            steps {
                script {
                    sh """#!/bin/bash
                       if ! command -v databricks &>/dev/null; then
                            echo Databricks CLI is not there
                            curl -fsSL https://raw.githubusercontent.com/databricks/setup-cli/main/install.sh | sudo sh
                            databricks --version
                            export DATABRICKS_HOST="https://adb-1772544086334040.0.azuredatabricks.net"
                            export DATABRICKS_TOKEN="dapi3e5cf647bb68838acfee404a8a362f42-3"
                            databricks configure -p  demo-dev
                            cd ~
                            sudo cat .databrickscfg
                        else
                            echo Databricks CLI is already installed.
                            databricks --version
                            export DATABRICKS_HOST="https://adb-1772544086334040.0.azuredatabricks.net"
                            export DATABRICKS_TOKEN="dapi3e5cf647bb68838acfee404a8a362f42-3"
                            databricks configure -p demo-dev-else
                            cd ~
                            sudo cat .databrickscfg
                        fi

                    """
                }
            }
        }

        stage('Validate Bundle') {
            steps {
                script {
                    sh """
                        cd dab_p2
                        ${DBCLIPATH}/databricks bundle -p demo validate -t ${BUNDLETARGET}
                    """
                }
            }
        }

        stage('Deploy Bundle') {
            steps {
                script {
                    sh """
                        cd dab_p2
                        ${DBCLIPATH}/databricks bundle -p demo deploy -t ${BUNDLETARGET}
                    """
                }
            }
        }
    }
}

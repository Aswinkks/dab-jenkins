pipeline {
    agent any

    environment {
        GITREPOREMOTE = "https://github.com/Aswinkks/dab-jenkins.git"
        GITBRANCH     = "main"
        DBCLIPATH     = "/usr/local/bin/"
        BUNDLETARGET  = "dev"
        DATABRICKS_HOST = credentials('DATABRICKS_HOST')
        DATABRICKS_TOKEN = credentials('DATABRICKS_TOKEN')
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
                            export DATABRICKS_HOST=${DATABRICKS_HOST}
                            export DATABRICKS_TOKEN=${DATABRICKS_TOKEN}
                            databricks configure --profile  demo-dev-new
                            cd ~
                            sudo cat .databrickscfg
                        else
                            echo Databricks CLI is already installed.
                            databricks --version
                            export DATABRICKS_HOST=${DATABRICKS_HOST}
                            export DATABRICKS_TOKEN=${DATABRICKS_TOKEN}
                            databricks configure --profile demo-dev-new
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
                        ${DBCLIPATH}/databricks bundle -p demo-dev-new validate -t ${BUNDLETARGET}
                    """
                }
            }
        }

        stage('Deploy Bundle') {
            steps {
                script {
                    sh """
                        cd dab_p2
                        ${DBCLIPATH}/databricks bundle -p demo-dev-new deploy -t ${BUNDLETARGET}
                    """
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        GITREPOREMOTE = "https://github.com/Aswinkks/dab-jenkins.git"
        GITBRANCH     = "main"
        DBCLIPATH     = "/opt/homebrew/bin"
        JQPATH        = "/opt/homebrew/bin"
        JOBPREFIX     = "jenkins-demo"
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
                    sh """
                        echo "============="
                        curl -fsSL https://raw.githubusercontent.com/databricks/setup-cli/main/install.sh | sudo sh
                        databricks --version
                        echo "--------------"
                    """
                }
            }
        }

        stage('Validate Bundle') {
            steps {
                script {
                    sh """
                        cd dab_p2
                        ${DBCLIPATH}/databricks bundle validate -t ${BUNDLETARGET}
                    """
                }
            }
        }

        stage('Deploy Bundle') {
            steps {
                script {
                    sh """
                        cd dab_p2
                        ${DBCLIPATH}/databricks bundle deploy -t ${BUNDLETARGET}
                    """
                }
            }
        }
    }
}

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
                    if ! command -v databricks &>/dev/null; then
                        echo "Databricks CLI not found. Downloading and configuring..."
                        sudo curl -fsSL https://raw.githubusercontent.com/databricks/setup-cli/main/install.sh | sh
                        databricks --version
                    else
                        echo "Databricks CLI is already installed."
                        databricks --version
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

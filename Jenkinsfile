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
                        // curl -fsSL https://raw.githubusercontent.com/databricks/setup-cli/main/install.sh | sudo sh
                        databricks --version
                        export DATABRICKS_HOST="https://adb-1772544086334040.0.azuredatabricks.net"
                        export DATABRICKS_TOKEN="dapi3e5cf647bb68838acfee404a8a362f42-3"
                        databricks configure
                        cd
                        sudo cat .databrickscfg
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

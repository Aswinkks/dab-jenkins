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
                    sh """
                        echo "============="
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

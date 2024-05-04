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

        // stage('Databricks Configure') {
        //     steps {
        //         // Add your Databricks configuration steps here
        //         // For example, setting up Databricks CLI or configuring Databricks connection
        //     }
        // }

        stage('Validate Bundle') {
            steps {
                script {
                    sh """
                        cd dab_p2
                        ${DBCLIPATH}/databricks bundle -p my-dev5 validate -t ${BUNDLETARGET}
                    """
                }
            }
        }

        stage('Deploy Bundle') {
            steps {
                script {
                    sh """
                        cd dab_p2
                        ${DBCLIPATH}/databricks bundle -p my-dev5 deploy -t ${BUNDLETARGET}
                    """
                }
            }
        }
    }
}

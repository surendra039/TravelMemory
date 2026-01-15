pipeline {
    agent any

    environment {
        // Set Python version if needed
        PYTHON_VERSION = "3.11"
        VENV_DIR = "venv"
    }

    stages {

        stage('Build') {
            steps {
                echo 'Setting up Python environment and installing dependencies...'

                // Create virtual environment
                sh "python${PYTHON_VERSION} -m venv ${VENV_DIR}"

                // Activate virtual environment and install requirements
                sh """
                source ${VENV_DIR}/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'

                // Run pytest inside virtual environment
                sh """
                source ${VENV_DIR}/bin/activate
                pytest --maxfail=1 --disable-warnings -q
                """
            }

            post {
                // Archive test results even if tests fail
                always {
                    junit '**/test-results/*.xml'  // optional if pytest produces XML
                }
            }
        }

        stage('Deploy') {
            when {
                expression {
                    // Only deploy if tests passed
                    currentBuild.currentResult == 'SUCCESS'
                }
            }
            steps {
                echo 'Deploying to staging environment...'

                // Example: run a deployment script
                sh """
                source ${VENV_DIR}/bin/activate
                bash deploy_staging.sh
                """
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh "rm -rf ${VENV_DIR}"
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}

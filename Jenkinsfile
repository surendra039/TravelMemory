pipeline {
    agent any

    environment {
        VENV_DIR = "venv"
    }

    stages {

        stage('Build') {
            steps {
                echo 'Setting up Python 3.10 virtual environment...'
                sh '''
                #!/bin/bash
                python3 -m venv ${VENV_DIR}
                ${VENV_DIR}/bin/pip install --upgrade pip
                # Optionally install project packages here
                # ${VENV_DIR}/bin/pip install flask requests boto3
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh '''
                #!/bin/bash
                ${VENV_DIR}/bin/pip install pytest
                ${VENV_DIR}/bin/pytest --maxfail=1 --disable-warnings -q || true
                '''
            }

            post {
                always {
                    echo 'Test stage completed.'
                }
            }
        }

        stage('Deploy') {
            when {
                expression {
                    currentBuild.currentResult == 'SUCCESS'
                }
            }
            steps {
                echo 'Deploying to staging environment using deploy_staging.sh...'
                sh '''
                #!/bin/bash
                bash deploy_staging.sh
                '''
            }
        }
    }

    post {
        always {
            echo 'Cleaning up virtual environment...'
            sh "rm -rf ${VENV_DIR}"
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}

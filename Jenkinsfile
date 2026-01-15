pipeline {
    agent any

    environment {
        VENV_DIR = "venv"
    }

    stages {

        stage('Build') {
            steps {
                echo 'Setting up Python 3.10 environment and installing dependencies...'
                sh '''
                #!/bin/bash
                # Create virtual environment using Python 3
                python3 -m venv ${VENV_DIR}
                
                # Upgrade pip and install dependencies
                ${VENV_DIR}/bin/pip install --upgrade pip
                ${VENV_DIR}/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests using pytest...'
                sh '''
                #!/bin/bash
                # Ensure pytest is installed in venv
                ${VENV_DIR}/bin/pip install pytest
                # Run tests
                ${VENV_DIR}/bin/pytest --maxfail=1 --disable-warnings -q
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
                echo 'Deploying to staging environment...'
                sh '''
                #!/bin/bash
                # Run your deployment script
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
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}

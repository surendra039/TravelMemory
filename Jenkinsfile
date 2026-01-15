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
                
                # Upgrade pip
                ${VENV_DIR}/bin/pip install --upgrade pip
                
                # Optionally install any packages your project needs
                # ${VENV_DIR}/bin/pip install flask pytest boto3  # example
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh '''
                #!/bin/bash
                # Ensure pytest is installed
                ${VENV_DIR}/bin/pip install pytest
                
                # Run tests
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
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}

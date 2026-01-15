pipeline {
    agent any

    environment {
        VENV = "${WORKSPACE}/venv"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                git branch: 'main', url: 'https://github.com/surendra039/TravelMemory.git'
            }
        }

        stage('Build') {
            steps {
                echo "Setting up Python environment and installing dependencies..."
                sh '''
                    python3 -m venv $VENV
                    source $VENV/bin/activate
                    pip install --upgrade pip
                    pip install -r backend/requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Running backend tests..."
                sh '''
                    source $VENV/bin/activate
                    pytest backend/tests
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying Flask backend..."
                sh '''
                    source $VENV/bin/activate
                    nohup python3 backend/app.py &
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully. Application deployed."
        }
        failure {
            echo "Pipeline failed. Check errors in Jenkins console."
        }
    }
}

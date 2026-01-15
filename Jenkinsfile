pipeline {
    agent any

    environment {
        VENV = "${WORKSPACE}/venv"  // Path for Python virtual environment
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
                    # Create virtual environment
                    python3 -m venv "$VENV"

                    # Activate virtual environment
                    . "$VENV/bin/activate"

                    # Upgrade pip
                    pip install --upgrade pip

                    # Install dependencies
                    pip install -r backend/requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Running backend tests..."
                sh '''
                    # Activate virtual environment
                    . "$VENV/bin/activate"

                    # Run pytest for backend
                    pytest backend/tests
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying backend Flask application..."
                sh '''
                    # Activate virtual environment
                    . "$VENV/bin/activate"

                    # Start backend app in background
                    nohup python3 backend/app.py &
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully. Application deployed!"
        }
        failure {
            echo "Pipeline failed. Check Jenkins console for details."
        }
    }
}

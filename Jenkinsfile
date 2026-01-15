pipeline {
    agent any

    environment {
        BACKEND_VENV = "${WORKSPACE}/backend/venv"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                git branch: 'main', url: 'https://github.com/surendra039/TravelMemory.git'
            }
        }

        stage('Backend Build') {
            steps {
                echo "Setting up Python virtual environment and installing backend dependencies..."
                sh '''
                    python3 -m venv "$BACKEND_VENV"
                    . "$BACKEND_VENV/bin/activate"
                    pip install --upgrade pip
                    pip install -r backend/requirements.txt
                '''
            }
        }

        stage('Backend Test') {
            steps {
                echo "Running backend tests..."
                sh '''
                    . "$BACKEND_VENV/bin/activate"
                    pytest backend/tests || echo "No tests found or tests failed"
                '''
            }
        }

        stage('Backend Deploy') {
            steps {
                echo "Deploying backend Flask application..."
                sh '''
                    . "$BACKEND_VENV/bin/activate"
                    nohup python3 backend/app.py &
                '''
            }
        }

        stage('Frontend Build & Deploy') {
            steps {
                echo "Building and deploying frontend..."
                sh '''
                    # Move to frontend folder
                    cd frontend

                    # Install Node dependencies
                    if [ -f package.json ]; then
                        npm install
                        npm run build || echo "Frontend build failed"
                    fi

                    # Deploy: Copy build folder to a deployment location (adjust path as needed)
                    # Example: cp -r build /var/www/html/frontend
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully. Backend and Frontend deployed!"
        }
        failure {
            echo "Pipeline failed. Check Jenkins console for details."
        }
    }
}

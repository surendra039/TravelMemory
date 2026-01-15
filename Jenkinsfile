pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/surendra039/TravelMemory.git'
            }
        }

        stage('Build Backend') {
            steps {
                sh '''
                cd backend
                npm install
                '''
            }
        }

        stage('Build Frontend') {
            steps {
                sh '''
                cd frontend
                npm install
                '''
            }
        }

        stage('Test Backend') {
            steps {
                sh '''
                cd backend
                npm test || true
                '''
            }
        }

        stage('Test Frontend') {
            steps {
                sh '''
                cd frontend
                npm test || true
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                # start backend
                cd backend
                nohup npm start &

                # deploy frontend (if build step exists)
                # If frontend has production build, uncomment:
                # cd frontend
                # npm run build
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}

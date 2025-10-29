pipeline {
    agent any

    stages {

        stage('Setup Python Environment') {
            steps {
                echo "Setting up Python environment"
                // Install dependencies
                bat '''
                    python --version
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                echo "Starting Flask application..."
                // Start Flask app in background
                bat 'start /B python app.py'

                echo "Waiting for Flask app to start..."
                // Sleep for 5 seconds safely (replaces ping)
                bat 'timeout /t 5 /nobreak >nul'
            }
        }

        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"
                bat '''
                    pytest --maxfail=1 --disable-warnings -q
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Build and Tests completed successfully!"
        }
        failure {
            echo "❌ Build failed. Check logs for errors."
        }
    }
}

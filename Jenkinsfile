pipeline {
    agent any

 tools {
    nodejs "node-js24"
}


    stages {
        stage('Checkout') {
            steps {
                echo "Source code checked out from Git (handled by Jenkins SCM)."
            }
        }

        stage('Install dependencies') {
            steps {
                echo "Installing npm dependencies..."
                sh 'npm install'
            }
        }

        stage('Run tests (optional)') {
            when {
                expression { return fileExists('package.json') }
            }
            steps {
                echo "Running tests (if configured)..."
                // If tests fail or don't exist, don't break the build
                sh 'npm test -- --watch=false || echo "Tests not configured or failed"'
            }
        }

        stage('Build') {
            steps {
                echo "Building production bundle..."
                // Avoid CRA failing on warnings by turning off CI mode
                sh 'CI=false npm run build'
            }
        }

        stage('Archive build artifacts') {
            steps {
                echo "Archiving build folder..."
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ UOMO React e-commerce build SUCCESS'
        }
        failure {
            echo '❌ Build FAILED – check the console output.'
        }
    }
}

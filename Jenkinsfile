pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Sanity Check') {
            steps {
                echo "Running sanity checks..."
                sh '''
                  echo "Checking files..."
                  test -f index.html
                  test -f style.css
                  echo "Sanity check passed ✅"
                '''
            }
        }

        stage('Build (Optional)') {
            steps {
                echo "Static site — no build required"
            }
        }
    }

    post {
        success {
            echo "✅ Jenkins pipeline succeeded"
        }
        failure {
            echo "❌ Jenkins pipeline failed"
        }
    }
}

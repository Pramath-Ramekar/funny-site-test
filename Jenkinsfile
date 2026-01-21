pipeline {
    agent any

    environment {
        REPO_URL = 'github.com/Pramath-Ramekar/funny-site-test.git'
        MAIN_BRANCH = 'main'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Sanity Check') {
            steps {
                sh '''
                  echo "Running sanity checks"
                  test -f index.html
                  test -f style.css
                  echo "Sanity check passed"
                '''
            }
        }

        stage('Auto Push to Main') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'github-push-token',
                    usernameVariable: 'GIT_USER',
                    passwordVariable: 'GIT_TOKEN'
                )]) {
                    sh '''
                      git config user.name "jenkins-bot"
                      git config user.email "jenkins@ci.local"

                      git fetch origin
                      git checkout main || git checkout -b main
                      git pull https://$GIT_USER:$GIT_TOKEN@$REPO_URL main

                      git merge origin/test/funny-site --no-edit

                      git push https://$GIT_USER:$GIT_TOKEN@$REPO_URL main
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Jenkins successfully promoted code to main"
        }
        failure {
            echo "❌ Jenkins failed — main untouched"
        }
    }
}

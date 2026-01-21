pipeline {
    agent any

    environment {
        GIT_CREDS = 'github-push-token'
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
                  test -f index.html
                  test -f style.css
                  echo "Sanity check passed"
                '''
            }
        }

        stage('Promote to Main') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: env.GIT_CREDS,
                    usernameVariable: 'GIT_USER',
                    passwordVariable: 'GIT_TOKEN'
                )]) {
                    sh '''
                      git config user.name "jenkins-bot"
                      git config user.email "jenkins@ci.local"

                      git checkout main || git checkout -b main
                      git pull https://$GIT_USER:$GIT_TOKEN@github.com/Pramath-Ramekar/funny-site-test.git main

                      git merge origin/test/funny-site --no-edit

                      git push https://$GIT_USER:$GIT_TOKEN@github.com/Pramath-Ramekar/funny-site-test.git main
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Auto-merged to main successfully"
        }
        failure {
            echo "❌ Build failed — main NOT touched"
        }
    }
}

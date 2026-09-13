pipeline {
    agent any

    environment {
        GIT_CREDENTIAL_ID = 'github_pat'
        GIT_USER_EMAIL = 'huuthien24@github.com'
        GIT_USER_NAME = 'huuthien24'
    }

    stages {
        stage('Checkout Source') {
            steps {
                git credentialsId: "${GIT_CREDENTIAL_ID}",
                    url: "https://github.com/huuthien24/node-web-demo.git",
                    branch: "main"
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Auto Update Version') {
            steps {
                sh '''
                    npm version patch --no-git-tag-version
                '''
            }
        }

        stage('Commit & Push Back to GitHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${GIT_CREDENTIAL_ID}", usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                    sh '''
                        git config user.email "${GIT_USER_EMAIL}"
                        git config user.name "${GIT_USER_NAME}"
                        
                        git add package.json package-lock.json
                        git commit -m "chore(ci): auto update version [skip ci]" || echo "No changes to commit"
                        
                        git push https://${GIT_USER}:${GIT_PASS}@github.com/huuthien24/node-web-demo.git HEAD:main
                    '''
                }
            }
        }
    }
}

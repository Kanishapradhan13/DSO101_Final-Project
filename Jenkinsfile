pipeline {
    agent any
    environment {
        // Store GitHub credentials in Jenkins Secrets
        GITHUB_CREDS = credentials('github credentials')
    }
    stages {
        stage('Check Commit Message') {
            steps {
                script {
                    // Check if commit message contains "@push"
                    def commitMsg = sh(returnStdout: true, script: 'git log -1 --pretty=%B').trim()
                    if (commitMsg.contains("@push")) {
                        echo "Triggering GitHub push..."
                    } else {
                        error("Commit message does not contain '@push'. Aborting.")
                    }
                }
            }
        }
        stage('Push to GitHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'github credentials',
                    usernameVariable: 'GITHUB_USER',
                    passwordVariable: 'GITHUB_TOKEN'
                )]) {
                    sh '''
                        git remote set-url origin https://${GITHUB_USER}:${GITHUB_TOKEN}@github.com/Kanishapradhan13/DSO101_Final-Project
                        git push origin HEAD:main
                    '''
                }
            }
        }
    }
}
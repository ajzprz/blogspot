pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/ajzprz/blogspot.git'
        GH_PAGES_BRANCH = 'gh-pages'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${GITHUB_REPO}"
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Deploy to GitHub Pages') {
            steps {
                script {
                    sh 'git checkout --orphan ${GH_PAGES_BRANCH}'
                    sh 'git reset --hard'
                    sh 'git commit --allow-empty -m "Deploying to GitHub Pages"'
                    sh 'git push origin ${GH_PAGES_BRANCH} --force'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

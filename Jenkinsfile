pipeline {
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                script {
                    bat 'if exist blogspot rmdir /s /q blogspot'
                }
                git(url: 'https://github.com/ajzprz/blogspot.git', branch: 'main')
            }
        }
        stage('Building Next.js project') {
            steps {
                bat 'npm install && npm run build' // Install dependencies and build Next.js project
            }
        }
        stage('Deploy to GitHub Pages') {
            steps {
                script {
                    bat 'if not exist gh-pages-temp mkdir gh-pages-temp'
                }
                bat 'xcopy /s /Y .\\out\\* gh-pages-temp'
                bat 'git config user.email "ajz.prz@gmail.com"'
                bat 'git config user.name "Ajaya Prajapati"'
                bat 'git checkout -B gh-pages'
                bat 'git add . && git commit -m "Deploy to GitHub Pages"'
                bat 'git push origin gh-pages --force'
                bat 'rmdir /s /q gh-pages-temp && git checkout main'
            }
        }
    }
}

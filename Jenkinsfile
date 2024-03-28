pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ajzprz/blogspot.git'
            }
        }
        
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Deploy to GitHub Pages') {
            steps {
                sh '''
                    git config --global user.email "ajz.prz@gmail.com"
                    git config --global user.name "ajzprz"
                    git checkout -b gh-pages
                    git add .
                    git commit -m "Deploy"
                    git push origin gh-pages
                '''
            }
        }
    }
}

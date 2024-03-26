pipeline {
    environment {
        imagename = "ajaya/blogpost"
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                script {
                    // Delete the existing directory if it exists
                    bat 'if exist blogspot rmdir /s /q blogspot'
                }
                git([url: 'https://github.com/ajzprz/blogspot.git', branch: 'main'])
            }
        }
        stage('Building Next.js project') {
            steps {
                bat 'npm install' // Install dependencies
                bat 'npm run build' // Build Next.js project
            }
        }
        stage('Deploy to GitHub Pages') {
            steps {
                script {
                    // Check if gh-pages-temp directory already exists
                    bat 'if not exist gh-pages-temp mkdir gh-pages-temp'
                    bat 'icacls gh-pages-temp /grant:r everyone:(OI)(CI)F' // Grant full access to everyone
                }

                // Debug statement to check the existence of the gh-pages-temp directory
                bat 'dir gh-pages-temp'

                // Copy static files to a temporary directory
                bat 'xcopy /s out gh-pages-temp'

                // Debug statement to check the content of the gh-pages-temp directory
                bat 'dir gh-pages-temp'

                // Introduce a delay to wait for Git operations to complete
                bat 'ping 127.0.0.1 -n 11 > nul'

                // Copy static files to the root directory
                bat 'xcopy /s /E /Y gh-pages-temp .'

                // Add, commit, and push to GitHub Pages branch
                bat 'git add .'
                bat 'git commit -m "Deploy to GitHub Pages"'
                bat 'git push origin gh-pages --force'

                // Clean up temporary files and switch back to main branch
                bat 'rmdir /s /q gh-pages-temp'
                bat 'git checkout main'
            }
        }
    }
}

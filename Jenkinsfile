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

                // Copy static files to a temporary directory
                bat 'xcopy /s /Y out gh-pages-temp'

                // Exclude node_modules from being added to the repository
                bat 'echo node_modules/ >> .gitignore'

                // Configure Git user email and name
                bat 'git config user.email "ajz.prz@gmail.com"'
                bat 'git config user.name "Ajaya Prajapati"'

                // Check for uncommitted changes
                bat 'git diff --exit-code || git add . && git commit -m "Temporary commit"'

                // Delete gh-pages branch if it exists
                bat 'git branch -D gh-pages || true'

                // Create gh-pages branch
                bat 'git checkout -b gh-pages'

                // Clear contents of gh-pages branch
                bat 'git rm -rf .'

                // Commit empty state
                bat 'git commit -m "Initial empty commit for GitHub Pages"'

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

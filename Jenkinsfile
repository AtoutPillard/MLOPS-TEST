pipeline {
    agent any
    stages {
        stage('Building and unit testing'){
            steps{
                bat "git checkout ${env.GIT_BRANCH}"
                dir('backend_rating') {
                    bat 'pip install -r requirements.txt'
                }
            }
        }
        stage('Push to Develop') {
            
            steps {
                bat 'git checkout dev'
                bat 'git pull origin dev'
                bat "git merge ${env.GIT_BRANCH}"
                bat 'git push origin dev'
                bat "git branch -d ${env.GIT_BRANCH}"
            }
        }
        stage('User Acceptance') {
            steps {
                input message {
                    "Proceed to push to main"
                }
            }
        }
        stage('Pushing to main') {
            steps {
                bat 'git checkout main'
                bat 'git pull origin main'
                bat 'git merge dev'
                bat 'git push origin main'
            }
        }
        stage('Pushing to Dockerhub') {
            steps {
                bat 'git checkout main'
                dir('backend_rating') {
                    bat 'docker build -t prediction_api .'
                    bat 'docker run -p 5000:5000 prediction_api'
                }
                bat 'docker-compose up'
            }
        }
    }
}
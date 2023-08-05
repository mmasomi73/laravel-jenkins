pipeline {
    agent any

    stages {
        stage('SCM Checkout') {
            steps {
                // here fetch main branch from git
                git branch: 'main', url: 'https://github.com/mmasomi73/laravel-jenkins.git'
            }
        }
        stage('Test'){
            steps{
                // here first run docker file
                sh "docker-compose up -d"
            }
        }
        stage('Build'){
            steps{
                echo 'Build Project'
            }
        }
        stage('Deploy'){
            steps{
                echo 'Deploy Project'
            }
        }
    }
}

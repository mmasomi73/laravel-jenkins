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
                echo 'Test Projects'
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

pipeline {
    agent any

    stages {
        stage('SCM Dev Pull') {
            steps {
                // here fetch main branch from git
                git branch: 'main', url: 'https://github.com/mmasomi73/laravel-jenkins.git'
            }
        }
        stage('Test'){
            steps{
                sh "composer install"
                sh "npm install"
            }
        }
        stage('Build'){
            steps{
                echo 'npm run build'
            }
        }
        stage('Production'){
            steps{
                dir('/var/www/laravel-jenkins'){
                    sh 'php artisan down'
                    sh "git reset --hard HEAD"
                    sh "git pull origin main"
                    sh "composer install"
                    sh "npm install"
                    sh "npm run build"
                    sh "php artisan up"
                }

            }
        }
    }
}

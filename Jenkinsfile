pipeline {
    agent any

    environment {
        APP_NAME = "todo-app"
        PM2 = "/usr/bin/pm2"
    }

    stages {

        stage('Kill Old App') {
            steps {
                sh '''
                $PM2 delete $APP_NAME || true
                '''
            }
        }

        stage('Start App') {
            steps {
                sh '''
                $PM2 start server.js --name $APP_NAME
                $PM2 save
                '''
            }
        }
    }
}

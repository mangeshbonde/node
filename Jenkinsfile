pipeline {
    agent any

    environment {
        APP_NAME = "todo-app"
    }

    stages {

        stage('Kill Old App') {
            steps {
                sh '''
                if pm2 list | grep $APP_NAME; then
                    pm2 delete $APP_NAME
                fi
                '''
            }
        }

        stage('Start App') {
            steps {
                sh '''
                pm2 start server.js --name $APP_NAME
                pm2 save
                '''
            }
        }

    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}

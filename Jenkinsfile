pipeline {
    agent any

    environment {
        APP_NAME = "todo-app"
    }

    stages {

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
                sh 'npm install'
            }
        }

        stage('Stop Old App') {
            steps {
                sh 'pm2 delete $APP_NAME || true'
            }
        }

        stage('Start Application') {
            steps {
                sh 'pm2 start server.js --name $APP_NAME'
                sh 'pm2 save'
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

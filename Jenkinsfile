pipeline {
    agent any

    environment {
        APP_NAME = "node-todo-app"
        APP_PORT = "3000"
    }

    stages {

        stage('Setup Environment') {
            steps {
                sh '''
                set +e

                echo "===== CHECKING NODE ====="
                if ! command -v node >/dev/null 2>&1; then
                    sudo apt update -y
                    sudo apt install -y curl
                    curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
                    sudo apt install -y nodejs
                fi
                node -v
                npm -v

                echo "===== CHECKING PM2 ====="
                if ! command -v pm2 >/dev/null 2>&1; then
                    sudo npm install -g pm2
                fi
                pm2 -v
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                set +e

                echo "===== MOVING TO WORKSPACE ====="
                cd /var/lib/jenkins/workspace/project-2

                echo "===== STOPPING OLD APP ====="
                pm2 delete ${APP_NAME} || true

                echo "===== STARTING APP ====="
                pm2 start server.js --name ${APP_NAME}

                pm2 save

                echo "===== PM2 STATUS ====="
                pm2 list
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                sleep 3
                curl -I --max-time 5 http://localhost:${APP_PORT}
                '''
            }
        }
    }

    post {
        success {
            echo ""
            echo "======================================="
            echo "🎉 BUILD SUCCESSFUL 🎉"
            echo "Application is LIVE"
            echo "http://YOUR_PUBLIC_IP:3000"
            echo "======================================="
        }

        always {
            echo "===== PIPELINE FINISHED ====="
        }
    }
}

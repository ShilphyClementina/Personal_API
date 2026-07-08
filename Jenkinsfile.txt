pipeline {
    agent any

    tools {
        nodejs 'NodeJS_18'   // must match the NodeJS tool name you configured in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Newman') {
            steps {
                bat 'npm install -g newman'
            }
        }

        stage('Run Newman Tests') {
            steps {
                bat 'newman run "Users API.postman_collection.json" -e "TEST1.postman_environment.json"'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
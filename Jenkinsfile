pipeline {
    agent any

    tools {
        nodejs 'NodeJS_18'   // must match your NodeJS tool name in Jenkins
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
                // Run Newman and export results in JUnit + HTML format
                bat '''
                newman run "Users API.postman_collection.json" -e "TEST1.postman_environment.json" ^
                  --reporters cli,junit,html ^
                  --reporter-junit-export results.xml ^
                  --reporter-html-export results.html
                '''
            }
        }

        stage('Publish Reports') {
            steps {
                // Publish JUnit results
                junit 'results.xml'

                // Archive HTML report so you can download/view it
                archiveArtifacts artifacts: 'results.html', fingerprint: true
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
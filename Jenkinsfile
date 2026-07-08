pipeline {
    agent any
    tools {
        nodejs 'NodeJS_18'
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
                bat '''
                npx newman run "Users API.postman_collection.json" -e "TEST1.postman_environment.json" ^
                  --reporters cli,junit,html ^
                  --reporter-junit-export "results/results.xml" ^
                  --reporter-html-export "results/results.html"
                '''
            }
        }
        stage('Publish Reports') {
            steps {
                junit 'results/results.xml'
                archiveArtifacts artifacts: 'results/results.html', fingerprint: true
            }
        }
    }
}
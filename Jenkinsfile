pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                // Explicit checkout of full repo
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/ShilphyClementina/Personal_API.git']]
                ])

                // Debug: list files to confirm environment.json is present
                bat 'dir'
            }
        }

        stage('Install Newman & Reporters') {
            steps {
                bat 'npm install -g newman newman-reporter-html'
            }
        }

        stage('Run Newman Tests') {
            steps {
                // Ensure results directory exists
                bat 'mkdir results'

                // Run collection with environment file
                bat '''
                    npx newman run "Users API.postman_collection.json" ^
                        -e "TEST1.postman_environment.json" ^
                        --reporters cli,junit,html ^
                        --reporter-junit-export "results/results.xml" ^
                        --reporter-html-export "results/results.html"
                '''
            }
        }

        stage('Publish Reports') {
            steps {
                junit 'results/results.xml'
                publishHTML([reportDir: 'results', reportFiles: 'results.html', reportName: 'API Test Report'])
            }
        }
    }
}

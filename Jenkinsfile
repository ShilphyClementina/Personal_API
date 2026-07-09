pipeline {
    agent any

    parameters {
        // Dropdown choice for environment file
        choice(name: 'ENV_FILE', choices: ['TEST1.postman_environment.json', 'TEST2.postman_environment.json'], description: 'Select Postman environment file')
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/ShilphyClementina/Personal_API.git']]
                ])
                // Debug: confirm files are present
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
                bat 'if not exist results mkdir results'

                // Debug: show selected environment file contents
                bat "type %ENV_FILE%"

                // Run Newman in one line (Windows-safe)
                bat "npx newman run \"Users API.postman_collection.json\" -e \"%ENV_FILE%\" --reporters cli,junit,html --reporter-junit-export \"results/results.xml\" --reporter-html-export \"results/results.html\""
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

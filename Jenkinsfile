pipeline {
    agent any

    stages {
        stage('Start Grid') {
            steps {
                bat 'docker-compose -f grid.yaml up -d'
            }
        }
        stage('Run Test') {
            steps {
                bat 'docker-compose -f test-suites.yaml up --exit-code-from test'
            }
        }
        
        post {
            always {
                bat 'docker-compose -f grid.yaml down'
                bat 'docker-compose -f test-suites.yaml down'
            }
        }
    }
}
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
                archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: true
                archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: true
            }
        }
    }
}
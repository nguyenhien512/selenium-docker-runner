pipeline {
    agent any

    parameters {
        choice choices ['chrome', 'firefox'], description: 'Select browser to run the test', name: 'BROWSER'
    }

    stages {
        stage('Start Grid') {
            steps {
                bat "docker-compose -f grid.yaml --scale ${params.BROWSER}=2 up -d"
            }
        }
        stage('Run Test') {
            steps {
                bat  "SET BROWSER=${params.BROWSER} && docker-compose -f test-suites.yaml up"
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
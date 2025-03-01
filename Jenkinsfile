pipeline {
    agent any

    parameters {
        choice(name: 'BROWSER', choices: ['chrome', 'firefox'], description: 'Select browser to run the test')
    }

    stages {
        stage('Start Grid') {
            steps {
                bat "docker-compose -f grid.yaml up --scale ${params.BROWSER}=2 -d"
            }
        }
        stage('Run Test') {
            steps {
                bat "SET BROWSER=${params.BROWSER} && docker-compose -f test-suites.yaml up --pull=always "
                script {
                    if (fileExists('output/flight-reservation/testng-failed.xml') || fileExists('output/vendor-portal/testng-failed.xml')) {
                        error 'Test failed'
                    }
                }
            }
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
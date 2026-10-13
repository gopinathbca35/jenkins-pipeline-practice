pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code is available in the workspace'
                checkout scm
            }
        }

        stage('Validate') {
            steps {
                sh 'test -f README.md'
                sh 'echo "README file found"'
            }
        }

        stage('Build') {
            steps {
                sh '''
                    mkdir -p output
                    echo "Jenkins Pipeline Build" > output/build-info.txt
                    date >> output/build-info.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh 'test -s output/build-info.txt'
                echo 'Build artifact validation passed'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'output/**',
                                 fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'All Pipeline stages passed'
        }

        failure {
            echo 'Pipeline failed; review the failed stage'
        }

        always {
            echo 'Pipeline finished'
        }
    }
}

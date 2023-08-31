pipeline {
    agent none
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                dir('sources') {
                    sh 'python -m py_compile add2vals.py calc.py'
                }
            }
        }
        
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                dir('sources') {
                    sh 'py.test --verbose --junit-xml ../test-reports/results.xml test_calc.py'
                }
            }
            post {
                always {
                    junit '../test-reports/results.xml'
                }
            }
        }
        
        stage('Deliver') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                dir('sources') {
                    sh 'pip install pyinstaller'
                    sh 'pyinstaller --onefile add2vals.py'
                }
            }
            post {
                success {
                    archiveArtifacts allowEmptyArchive: true, artifacts: '../dist/*'
                }
            }
        }
    }
}

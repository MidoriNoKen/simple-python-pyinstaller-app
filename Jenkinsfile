node {
    def pythonImage = 'python:3-alpine'

    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        dir('sources') {
            docker.image(pythonImage).inside {
                sh 'pip install pyinstaller'
                sh 'pyinstaller --onefile add2vals.py'
            }
        }
    }

    stage('Test') {
        dir('sources') {
            docker.image(pythonImage).inside {
                sh 'pip install qnib/pytest'
                sh 'py.test --verbose --junit-xml ../test-reports/results.xml test_calc.py'
            }
        }
        junit 'test-reports/results.xml'
    }

    stage('Deliver') {
        dir('sources') {
            docker.image(pythonImage).inside {
                archiveArtifacts allowEmptyArchive: true, artifacts: '../dist/*'
            }
        }
    }
}

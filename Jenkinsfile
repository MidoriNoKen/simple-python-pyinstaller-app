node {
    def pythonImage = 'python:3-alpine'

    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        docker.image(pythonImage).inside {
            sh 'pip install pyinstaller'
            sh 'pyinstaller --onefile sources/add2vals.py'
        }
    }

    stage('Test') {
        docker.image(pythonImage).inside {
            sh 'pip install qnib/pytest'
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        junit 'test-reports/results.xml'
    }

    stage('Deliver') {
        docker.image(pythonImage).inside {
            archiveArtifacts allowEmptyArchive: true, artifacts: 'dist/*'
        }
    }
}

node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }

    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        post {
            always {
                junit 'test-reports/results.xml'
            }
        }
    }

    stage('Deliver') {
        steps {
            script {
                def pyinstallerImage = docker.image('cdrx/pyinstaller-linux:python2')
                def buildContainer = pyinstallerImage.run("-v ${pwd()}/sources:/src -w /src -i /bin/pyinstaller -o /src/dist /bin/sh -c 'pyinstaller --onefile add2vals.py'")
                def logs = buildContainer.inclCmd('sh', '-c', 'cat /src/dist/add2vals')
                archiveArtifacts artifacts: 'sources/dist/add2vals', allowEmptyArchive: true
                buildContainer.stop()
            }
        }
    }
}

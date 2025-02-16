node {
    def py
    def pyTest
    def pyInstaller

    stage("Pull Images") {
        py = docker.image('python:2-alpine')
        pyTest = docker.image('qnib/pytest')
        pyInstaller = docker.image('cdrx/pyinstaller-linux:python2')

        py.pull()
        pyTest.pull()
        pyInstaller.pull()
    }

    stage('Build') {
        py.inside {
            sh 'python -m py_compile ./sources/add2vals.py ./sources/calc.py'
        }
    }

    stage("Test") {
        pyTest.inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        junit 'test-reports/results.xml' // Process test results after the container exits
    }

    stage("Deliver") {
        docker.image('cdrx/pyinstaller-linux:python2').inside {
            sh 'pyinstaller --onefile sources/add2vals.py'
        }
        archiveArtifacts 'dist/add2vals'// Archive artifacts after the container exits
    }
}

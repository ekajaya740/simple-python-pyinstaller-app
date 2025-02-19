node {
    def py
    def pyTest

    stage("Pull Images") {
        py = docker.image('python:2-alpine')
        pyTest = docker.image('qnib/pytest')

        py.pull()
        pyTest.pull()
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

    def pyInstaller
    stage("Pull Deployment"){
        pyInstaller = docker.image('cdrx/pyinstaller-linux:python2')
        pyInstaller.pull()
    }

    stage("Deploy"){
        pyInstaller.inside {
          sh "pyinstaller --onefile sources/add2vals.py"
        }
        archiveArtifacts 'dist/add2vals'
    }
}

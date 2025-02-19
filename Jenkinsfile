node {
    stage("Pull Images") {
        // Pull images in advance (optional, but good practice)
        docker.image('python:2-alpine').pull()
        docker.image('qnib/pytest').pull()
        docker.image('cdrx/pyinstaller-linux:python2').pull()
    }

    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile ./sources/add2vals.py ./sources/calc.py'
        }
    }

    stage("Test") {
        docker.image('qnib/pytest').inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        junit 'test-reports/results.xml' // Process test results after the container exits
    }

    stage("Deploy") {
        docker.image('cdrx/pyinstaller-linux:python2').inside {
            sh "pyinstaller --onefile sources/add2vals.py"
        }
        archiveArtifacts 'dist/add2vals'
    }
}

node {
    def py
    def pyTest

    stage("Pull Images") {
        py = docker.image('python:3-alpine')
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

    stage("Manual Approval") {
        input message: 'Lanjutkan ke tahap Deploy?'

    }

    stage("Deploy"){
        py.inside("-u root") {
          sh "apk add binutils"
          sh "pip install pyinstaller"
          sh "pyinstaller --onefile sources/add2vals.py"
          sleep time: 1, unit: 'MINUTES'
          echo 'Pipeline has finished successfully.'
        }
        archiveArtifacts 'dist/add2vals'
    }

    stage("Send to EC2"){
      sshPublisher(
        publishers: [
                        sshPublisherDesc(
                            configName: 'my-ec2-instance',
                            transfers: [
                                sshTransfer(
                                    sourceFiles: 'dist/add2vals*',
                                    removePrefix: 'dist/',
                                    remoteDirectory: '/dicoding', 
                                )
                            ],
                            usePromotionTimestamp: false,
                            verbose: true
                        )
                    ]
      )
    }
}

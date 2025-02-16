node {
  def py
  def pyTest
  def pyInstaller
  stage("Pull Images"){
    py = docker.image('python:2-alpine')
    pyTest = docker.image('qnib/pytest')
    pyInstaller = docker.image('cdrx/pyinstaller-linux:python2')

    py.pull()
    pyTest.pull()
    pyInstaller.pull()
  }
  stage('Build'){
    py.inside{
      sh 'python -m py_compile ./sources/add2vals.py ./sources/calc.py'
    }
  }
  stage("Test"){

    pyTest.inside{
        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
    }
  }
}

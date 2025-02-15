node {
  def image
  stage("Pull Image"){
    image = docker.image('python:2-alpine')
    image.pull()
  }
  stage('Build'){
    image.inside(){
      sh 'python -m py_compile ./sources/add2vals.py ./sources/calc.py'
    }
  }
 }

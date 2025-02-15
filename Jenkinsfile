node {
  def image
  stage("Pull Image"){
    image = docker.image('node:16-buster-slim')
    image.pull()
  }
  stage('Build'){
    image.inside(){
      sh 'python -m py_compile sources/add2vals.py sources/calc.py'
    }
  }
 }

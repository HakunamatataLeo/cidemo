pipeline {
  agent none
  stages {
    stage('build and test') {
      agent none
      stages {
        stage('build'){
          steps {
            bat 'C:/ProgramData/DockerDesktop/version-bin/docker.exe pull python:3.6.9-alpine'
            sh 'pip install --no-cache-dir -r requirements.txt'
          }
        }
        stage('test') {
          steps {
            sh 'python test.py'
          }
          post {
            always {
              junit 'test-reports/*.xml'
            }
          }
        }
      }
    }
    stage('build docker image'){
      agent any
      steps{
        sh 'docker build -t my-flask-image:latest .'
        sh 'a=`docker images -f "dangling=true" -q | wc -l`'
        sh 'if [ $a -ge 0 ];then docker rmi $(docker images -f "dangling=true" -q);fi'
      }
    }
  }
}
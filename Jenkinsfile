pipeline {
  def app
  agent {
    docker {
      image 'maven:3.5-jdk-7'
      args '-v /d/SIGA_DOCKER/Public/.m2:/root/.m2'
    }

  }
  stages {
    stage('Build App') {
      steps {
        sh 'mvn clean install -Dmaven.test.skip=true'
      }
    }
    stage('Prep Docker Image') {
      steps {
        dir(path: 'SigaDockerImage') {
          git 'http://172.17.0.4:3000/diogo/Docker.git'
        }

        sh 'cp -r ./target SigaDockerImage/deploy'
      }
    }
    stage('Image Build') {
      steps {
        dir(path: 'SigaDockerImage'){
          app = docker.build("sigadoc:dev")
        }
      }
    }
  }
}

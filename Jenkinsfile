pipeline {
  agent any

  environment {
    CONTAINER_NAME = "nestjs-app"
    IMAGE_NAME = "nestjs-image"
    EMAIL = "rana.m.k422@gmail.com"
    PORT = "3000"
  }

  stages {
    stage('clone repository') {
      steps{
          git branch: 'main', url: 'git@github.com:aslamaswed/nestjs-app-server-22-local-with-jenkins-docker.git'
      }
    }

    stage('build docker image') {
      steps {
        script {
          sh 'docker build -t $IMAGE_NAME .'
        }
      }
    }
     stage('stop & remove previous container') {
      steps {
        script {
          sh '''
            docker stop $CONTAINER_NAME || true
            docker rm $CONTAINER_NAME || true
          '''
        }
      }
    }
     stage('Docker Container Run ') {
      steps {
        script {
          sh '''
            docker run -d -p $PORT:$PORT --name $CONTAINER_NAME $IMAGE_NAME
          '''
        }
      }
    }
    stage('Send Email Notification') {
      steps {
        script {
         emailext(
          subject: "Nestjs App Deployed Successful",
          body: "Your Nestjs application has been successfully deployed and is running on  http://192.168.100.7:${PORT}.",
          to: "${EMAIL}"
         )
        }
      }
    }
  }
}
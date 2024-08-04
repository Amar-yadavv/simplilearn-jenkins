pipeline {
    agent any

    stages {
        stage('Code Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Amar-yadavv/simplilearn-jenkins.git']])
            }
        }
        stage ('Build Image') {
            steps {
             sh 'docker image build -t amar1997/todo-application:v$BUILD_ID .'
             sh 'docker tag amar1997/todo-application:v$BUILD_ID amar1997/todo-application:latest'
          }
        }
        stage ('Pushing Image') {
            steps {
             withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'password', usernameVariable: 'username')]) {
              sh 'docker login -u ${username} -p ${password}'
              sh 'docker image push amar1997/todo-application:v$BUILD_ID'
              sh 'docker image push amar1997/todo-application:latest'
            }
          }
        }
        stage('Deployment') {
         steps {
             sh 'docker container run -it -d -p 3000:3000 --name todoapplication amar1997/todo-application:latest'
          }
        }
    }
}


pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf dockertest1'
            sh 'git clone https://github.com/mohankrishna110/dockertest1.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd /var/lib/jenkins/workspace/pipeline2'
            sh 'cp  /var/lib/jenkins/workspace/pipeline2/dockertest1/* /var/lib/jenkins/workspace/pipeline2'
            sh 'docker build -t mohan110/feature2:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push mohan110/feature2:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.1.1.181:2375 stop feature 2 || true'
            sh    'docker -H tcp://10.1.1.181:2375 run --rm -dit --name feature2 --hostname feature2 -p 8200:80 mohan110/feature2:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://10.1.1.181:8200'
          }
        }

    }
}

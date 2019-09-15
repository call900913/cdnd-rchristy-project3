pipeline {
  agent any
  stages {
    stage('Lint') {
      steps {
        sh 'tidy -q -e index.html'
      }
    }
    stage('Build Image') {
      steps {
        sh 'sudo docker image build -t call900913/udacity-frontend:latest udacity-c3-frontend'
        sh 'sudo docker image build -t call900913/reverseproxy:latest udacity-c3-deployment/docker'
        sh 'sudo docker image build -t call900913/udacity-restapi-user:latest udacity-c3-restapi-user'
        sh 'sudo docker image build -t call900913/udacity-restapi-feed:latest udacity-c3-restapi-feed'
      }
    }
    stage('Push Image') {
      steps {
        script {
          docker.withRegistry( '', registryCredential ) {
            sh 'docker push call900913/udacity-frontend:latest'
            sh 'docker push call900913/reverseproxy:latest'
            sh 'docker push call900913/udacity-restapi-user:latest'
            sh 'docker push call900913/udacity-restapi-feed:latest'
          }
        }
      }
    }
    stage('Run Container') {
      steps {
        dir("/var/lib/jenkins/workspace/cloud-rchristy-project3_master") {
          sh '''
            export PATH=/var/lib/jenkins:$PATH
            && kubectl apply -f cloud-rchristy-project3/udacity-c3-deployment/kubernetes/env-configmap.yaml
            && kubectl apply -f cloud-rchristy-project3/udacity-c3-deployment/kubernetes/env-secret.yaml
            && kubectl apply -f cloud-rchristy-project3/udacity-c3-deployment/kubernetes/aws-secret.yaml
            && kubectl apply -f cloud-rchristy-project3/udacity-c3-deployment/kubernetes/frontend.yaml
            && kubectl apply -f cloud-rchristy-project3/udacity-c3-deployment/kubernetes/reverseproxy.yaml
            && kubectl apply -f cloud-rchristy-project3/udacity-c3-deployment/kubernetes/backend-feed.yaml
            && kubectl apply -f cloud-rchristy-project3/udacity-c3-deployment/kubernetes/backend-user.yaml
            '''
        }
      }
    }
  }
  environment {
    registryCredential = 'call900913'
  }
}

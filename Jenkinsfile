pipeline {
  agent {
    kubernetes {
  label 'jenkins-agent-my-app'
  yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    component: ci
spec:
  containers:
  - name: python
    image: python:3.12
    command:
    - cat
    tty: true
  - name: docker
    image: docker
    command:
    - cat
    tty: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-sock
  volumes:
  - name: docker-sock
    hostPath:
      path: /var/run/docker.sock
"""
}

  }
 triggers {
        pollSCM(' * * * * * ')
    }

  }
  stages {
    stage('Install dependencies') {
      steps {
        container(name: 'python') {
          sh 'pip install -r requirements.txt'
        }

      }
    }

    stage('Test python') {
      steps {
        container(name: 'python') {
          sh 'python test.py'
        }

      }
    }
    stage('Build image') {
    steps {
        container('docker') {
            sh "docker build -t localhost:4000/pythonstest:latest ."
            sh "docker push localhost:4000/pythonstest:latest"
        }
    }
}
    stage('Deploy ') {
      steps {
        container('kubectl') {
          sh 'kubectl apply -f ./kubernetes/deployment.yaml'
          sh 'kubectl apply -f ./kubernetes/deployement.yaml'
        }
      }
    }

  }
  post {
    always {
      echo 'Pipeline terminé.'
    }

  }
  triggers {
    pollSCM(' * * * * * ')
  }

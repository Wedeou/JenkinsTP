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
"""
    }
  }
 triggers {
        pollSCM(' * * * * * ')
    }

  stages {
    stage('Install dependencies') {
      steps {
        container('python') {
          sh 'pip install -r requirements.txt'
        }
      }
    }
    stage('Test python') {
      steps {
        container('python') {
          sh 'python test.py'
        }
      }
    }
  }
  post {
    always {
      echo 'Pipeline terminé.'
    }
  }
}
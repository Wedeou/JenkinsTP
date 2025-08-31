pipeline {
  agent {
    kubernetes {
      label 'jenkins-agent-my-app'
      yaml '''
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
'''
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

  }
  post {
    always {
      echo 'Pipeline terminé.'
    }

  }
  triggers {
    pollSCM(' * * * * * ')
  }
}
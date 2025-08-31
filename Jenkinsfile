pipeline {
  agent {
    kubernetes {
<<<<<<< HEAD
  label 'jenkins-agent-my-app'
  yaml """
=======
      label 'jenkins-agent-my-app'
      yaml '''
>>>>>>> ef911cd09495af0045ce7012b427cd4a0db7ed0c
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
<<<<<<< HEAD
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
=======
'''
>>>>>>> ef911cd09495af0045ce7012b427cd4a0db7ed0c
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
<<<<<<< HEAD
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
=======
>>>>>>> ef911cd09495af0045ce7012b427cd4a0db7ed0c

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
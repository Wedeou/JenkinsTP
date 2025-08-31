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
    volumeMounts:
    - mountPath: /home/jenkins/agent
      name: workspace
  - name: docker
    image: docker
    command:
    - cat
    tty: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-sock
  - name: kubectl
    image: lachlanevenson/k8s-kubectl:latest
    command:
    - cat
    tty: true
  volumes:
  - name: docker-sock
    hostPath:
      path: /var/run/docker.sock
  - name: workspace
    emptyDir: {}
"""
        }
    }

    triggers {
        pollSCM(' * * * * * ')
    }

    stages {

        stage('Install dependencies') {
            steps {
                container(name: 'python') {
                    timeout(time: 5, unit: 'MINUTES') {
                        sh 'pip install --no-cache-dir -r requirements.txt'
                    }
                }
            }
        }

        stage('Test Python') {
            steps {
                container(name: 'python') {
                    sh 'python test.py'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                container('docker') {
                    sh 'docker build -t localhost:4000/pythonstest:latest .'
                    sh 'docker push localhost:4000/pythonstest:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                container('kubectl') {
                    sh 'kubectl apply -f ./kubernetes/deployment.yaml'
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

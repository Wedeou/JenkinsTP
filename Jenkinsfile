pipeline {
  agent any
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
        container(name: 'docker') {
          sh 'docker build -t localhost:4000/pythonstest:latest .'
          sh 'docker push localhost:4000/pythonstest:latest'
        }

      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        container(name: 'kubectl') {
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
  triggers {
    pollSCM(' * * * * * ')
  }
}
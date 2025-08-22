pipeline {
    agent any

    environment {
       KUBERNETES_NAMESPACE = "piyush"
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Piyushjangid01/odoo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image"
                sh 'docker build -t odoo .'
            }
        }

        stage('Docker Login and Push') {
            steps {
                echo "Logging into DockerHub and pushing image"
                sh 'docker login -u piyushj01 -p Piyush@0001'
                sh 'docker tag odoo piyushj01/odoo:latest'
                sh 'docker push piyushj01/odoo:latest'
            }
        }
          stage('Deploy to Kubernetes') {
    steps {
        // Apply Kubernetes manifests from repo root
        sh 'microk8s kubectl apply -f deployment.yaml -f service.yaml -n $KUBERNETES_NAMESPACE'
            }
        }
    } 
}

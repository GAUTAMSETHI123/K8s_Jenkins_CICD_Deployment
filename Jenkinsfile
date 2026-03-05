pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = "us-east-1"
    }

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'master', url: 'https://github.com/GAUTAMSETHI123/K8s_Jenkins_CICD_Deployment.git'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    sh 'docker build -t gautamsethi123/react-app:v1 .'
                }
            }
        }
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo "$PASS" | docker login -u "$USER" --password-stdin'
                }
            }
        }

        stage('Push') {
            steps {
                sh 'docker push gautamsethi123/react-app:v1'
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh 'kubectl config use-context arn:aws:eks:us-east-1:345115395542:cluster/Jenkins_EKS'
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}

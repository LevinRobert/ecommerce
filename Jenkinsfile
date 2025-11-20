pipeline {
    agent any

    stages {

        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker') {
                        sh "docker build -t levin16robert/main:latest ."
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker') {
                        sh "docker push levin16robert/main:latest"
                    }
                }
            }
        }

        stage('Deploy To Kubernetes') {
            steps {
                withKubeCredentials(kubectlCredentials: [[
                    caCertificate: '',
                    clusterName: 'EKS-1',
                    contextName: '',
                    credentialsId: 'k8-token',
                    namespace: 'webapps',
                    serverUrl: 'https://8DC1B1A2D8A33CE8EF32B02B982FD5B1.gr7.ap-south-1.eks.amazonaws.com'
                ]]) {
                    sh "kubectl apply -f deployment-service.yml"
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                withKubeCredentials(kubectlCredentials: [[
                    caCertificate: '',
                    clusterName: 'EKS-1',
                    contextName: '',
                    credentialsId: 'k8-token',
                    namespace: 'webapps',
                    serverUrl: 'https://8DC1B1A2D8A33CE8EF32B02B982FD5B1.gr7.ap-south-1.eks.amazonaws.com'
                ]]) {
                    sh "kubectl get svc -n webapps"
                }
            }
        }

    }
}

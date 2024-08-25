pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/jbn1995/Devops-automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t jabin95/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                    withCredentials([usernameColonPassword(credentialsId: 'dockerID', variable: 'dockerId')]) {
                    sh 'docker login -u jabin95 -p ${dockerID}'
                    
}
                    sh 'docker push jabin95/devops-integration'
                }
            }
        }
        stage('Deploy to k8s') {
            steps {
                script {
                    // Point kubectl to Minikube's context
                    sh 'kubectl config use-context minikube'

                    // Deploy the Kubernetes configurations
                    sh 'kubectl apply -f deploymentservice.yaml'
                }
            }
        }
    }
}

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
                    sh 'docker build -t jabin95/devops-integration:03 .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([usernamePassword(credentialsId: 'dockerID', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh 'docker login -u $USERNAME -p $PASSWORD'
                    }
                    sh 'docker push jabin95/devops-integration:03'
                }
            }
        }
        stage('Deploy to k8s') {
            steps {
                script {
                    // Point kubectl to Minikube's context
                    sh 'kubectl config use-context minikube'

                    // Deploy the Kubernetes configurations
                    sh 'kubectl apply -f deploymentservice.yaml --validate=false'
		    sh 'kubectl get services'
		    sh 'minikube service springboot-k8s --url'
                }
            }
        }
    }
}

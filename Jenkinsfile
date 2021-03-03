pipeline {
    environment {
        registry = "ryongg/flask-webapp"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    agent any
    stages {
        stage ('Build docker image'){
            steps {
                script {
                    dockerImage = docker.build registry + ":latest"
                    // dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage ('Push image to Docker Hub'){
            steps {
                script {
                    docker.withRegistry( '', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage ('Remove unused image'){
            steps {
                // sh "docker rmi $registry:$BUILD_NUMBER"
                sh "docker rmi $registry:latest"

            }
        }

        stage ('Delete all existing k8s services'){
            steps {
                sh "microk8s kubectl delete services --all"
            }
        }

        stage ('Delete all existing k8s deployments'){
            steps {
                sh "microk8s kubectl delete deployments --all"
            }
        }

        stage ('Deploy in Microk8s'){
            steps {
                sh "microk8s kubectl create -f kubernetes.yaml"
            }
        }
    }
}
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
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
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
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }

        stage ('Deploy in Microk8s'){
            steps {
                sh "kubectl create -f kubernetes.yaml"
            }
        }
    }
}
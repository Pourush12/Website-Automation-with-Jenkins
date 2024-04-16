pipeline{
    agent any
    stages{
        stage('Delete existing docker image and container'){
            steps{
            checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/herambh17/website']])
            script{
            bat 'docker rmi -f pourush123/html:latest'
            bat 'docker rm -f pourush123/html:latest'
            }
            }
        }
        stage('Build Docker image'){
            steps{
            script{
            bat 'docker build -t pourush123/html .'
            }
            }
        }
        stage('Push image to Docker Repository'){
            steps{
            script{
            bat 'docker login -u "pourush123" -p "830647@Sh" docker.io'
            bat 'docker push pourush123/html:latest'
            }
            }
        }
        stage('Starting Minikuber Cluster'){
            steps{
                script{
                    bat 'minikube start --driver=docker'
            }
        }
        stage('Creating Depoyments in Minikube Cluster'){
            steps{
                script{
                    bat 'echo '
            }
        }
    }
}
}

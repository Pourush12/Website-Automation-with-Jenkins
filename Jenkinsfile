pipeline{
    agent any
    stages{
        stage('Delete existing docker image and container'){
            steps{
            checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Pourush12/Website-Automation-with-Jenkins']])
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
        stage('Checking if the deployment exists') {
            steps {
                script {
                    def svcOutput = bat(script: 'kubectl get svc html', returnStdout: true).trim()
                    def deploymentOutput = bat(script: 'kubectl get deployment html', returnStdout: true).trim()

                    if (svcOutput.isEmpty()) {
                        echo 'Service "html" does not exist'
                    } else {
                        bat 'kubectl delete svc html'
                        echo 'Service "html" deleted'
                    }

                    if (deploymentOutput.isEmpty()) {
                        echo 'Deployment "html" does not exist'
                    } else {
                        bat 'kubectl delete deployment html'
                        echo 'Deployment "html" deleted'
                    }
                }
            }
        }
        stage('Creating Depoyments in Minikube Cluster'){
            steps{
                script{
                    bat 'kubectl create deployment html --image=pourush123/html:latest'
            }
        }
        stage('Exposing the deployment to NodePort'){
            steps{
                script{
                    bat 'kubectl expose deployment html --type=NodePort --port=80'
            }
        }
    }
}
}

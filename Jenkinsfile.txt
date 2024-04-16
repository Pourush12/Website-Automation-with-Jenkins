pipeline{
    agent any
    stages{
        stage('Delete existing docker image and container'){
            steps{
            checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/herambh17/website']])
            script{
            bat 'docker rmi -f herambhkumawat17/website:latest'
            bat 'docker rm -f herambhkumawat17/website:latest'
            }
            }
        }
        stage('Build Docker image'){
            steps{
            script{
            bat 'docker build -t herambhkumawat17/website .'
            }
            }
        }
        stage('Push image to Kubernets cluster'){
            steps{
            script{
            bat 'docker login -u "herambhkumawat17" -p "Herkum@17" docker.io'
            bat 'docker push herambhkumawat17/website:latest'
            }
            }
        }
        stage('Creating new Container'){
            steps{
                script{
                    bat 'docker run -it -d -P --name=cont_html herambhkumawat17/website'
            }
        }
    }
}
}

pipeline {
    agent any
    environment{
        dockerhub=credentials('dockerhub')
    }
    
    stages {
        stage('Git Checkout....') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GitHub', url: 'https://github.com/mayurjadhav591989/jenkins-nodejs-docker-k8s.git']]])
            }
        }
        
        stage('Build Docker Image....') {
            steps {
                sh 'docker build -t mayurjadhav1989/customimages:latest . '
            }
        }
        
        stage('Publish Artifacts To Dockerhub....') {
            steps {
                sh 'docker image ls'
                sh 'docker logout'
                sh 'echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin docker.io'
                sh 'docker push mayurjadhav1989/customimages:latest'
            }
        }
        stage('Deploying App to Kubernetes') {
            steps {
                script {
                     kubernetesDeploy(configs: "nodejs-app-deploy.yml", kubeconfigId: "kubecreds")
                       }
                  }
        }
    }
}

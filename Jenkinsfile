def dockerRun = "docker run -d -p 8081:8080 ambarodzich/docker-app:v2"

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/AMBarodzich/lesson8']])    
            }
        }
        stage('Maven-build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t ambarodzich/docker-app:v2 .'
            }
        }
        stage('Docker Push') {
            steps {
                withCredentials([string(credentialsId: 'DockerHubPWD', variable: 'DockerHubPWD')]) {
                    sh "docker login -u ambarodzich -p ${DockerHubPWD}"
                }
                sh "docker push ambarodzich/docker-app:v2"
            }
        }
        stage('Deploy') {
            steps {
                sshagent(['ubuntu']) {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.50.123 ${dockerRun}"
                }    
            }
        }
    }
}

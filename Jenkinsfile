def dockerRun = "docker run -d -p 8081:8080 ambarodzich/docker-app:'$BUILD_NUMBER'"

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/AMBarodzich/lesson8.git']])
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('docker_build') {
            steps {
                sh 'docker build -t ambarodzich/docker-app:"$BUILD_NUMBER" .'
            }
        }
        stage('docker_push') {
            steps {
                withCredentials([string(credentialsId: 'DockerHubPwd', variable: 'DockerHubPwd')]) {
                   sh 'docker login -u ambarodzich -p ${DockerHubPwd}' 
                }
                sh 'docker push ambarodzich/docker-app:"$BUILD_NUMBER"'
            }
        }
        stage('docker_deploy') {
            steps {
                sshagent(['b5960653-0f8b-4bd5-a706-768517b79d23']) {
                    sh  "ssh -o StrictHostkeyChecking=no ubuntu@172.31.19.170 ${dockerRun}"  
                }
            }
        }
    }
}    

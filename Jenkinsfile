pipeline {
	agent any

	stages {
		stage('CheckOut') {
			steps {
				checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kallinnas/devops_lesson8']])
			}
		}
		
		stage('Maven-build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Docker Build') {
            steps {
		        sh 'docker build -t kallinnas/jenkins-docker-app:v1 .'
            }
        }
        
        stage('Docker Push') {
            steps {
		        withCredentials([string(credentialsId: 'DockerHubCredentials', variable: 'DockerHubCredentials')]) {
			        sh "docker login -u kallinnas -p ${DockerHubCredentials}"
		        }
		        sh "docker push kallinnas/jenkins-docker-app:v1"
            }
        }
        
        stage('Docker Deploy') {
            steps {
                script {
                    def dockerRun = "docker run -d -p 8080:8080 kallinnas/jenkins-docker-app:v1"
                    sshagent(['ubuntu']) {
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.19.184 ${dockerRun}"
                    }
                }
            }
        }
	}
}

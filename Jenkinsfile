node {
    stage('Checkout') {
        git branch: 'main', url: 'https://github.com/AMBarodzich/lesson8'
    }
    stage('Maven Package') {
        sh 'mvn clean package'
    }
    stage('Docker Build Image') {
        sh 'docker build -t ambarodzich/docker-app:2.0 .'
    }
    stage('Docker Push') {
        withCredentials([string(credentialsId: 'DockerHubPwd', variable: 'DockerHubPWD')]) {
            sh "docker login -u ambarodzich -p ${DockerHubPWD}"
        }
        sh "docker push ambarodzich/docker-app:2.0"
    }
    stage('Deploy') {
        def dockerRun = "docker run -d -p 8080:8080 ambarodzich/docker-app:2.0"
        sshagent(['new_ubuntu']) {
            sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.84.174 ${dockerRun}"
        }       
    }
}

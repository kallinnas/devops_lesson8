node {
    stage('Checkout'){
        git branch: 'main', url: 'https://github.com/AMBarodzich/lesson8'
    }
    stage('Maven Package'){
        sh 'mvn clean package'
    }
    stage('Docker Build Image'){
        sh 'docker build -t ambarodzich/docker-app:1.0 .'
    }
    stage('Docker Push'){
        withCredentials([string(credentialsId: 'DockerHubPwd', variable: 'DockerHubPwd')]) {
            sh "docker login -u ambarodzich -p ${DockerHubPwd}"
        } 
        sh 'docker push ambarodzich/docker-app:1.0'
    }
    stage('Run container'){
        def dockerRun = 'docker run -p 8080:8080 -d --name docker-app ambarodzich/docker-app:1.0'
        sshagent(['new_ubuntu']) {
            sh "ssh -o StrictHostKeyChecking=no jenkins@172.31.81.170 ${dockerRun}"
        }
    }
}

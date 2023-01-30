pipeline {
    environment {
        imagename = "ranjitdas977/jenkins_cicd"
        registryCredential = "jenkins_docker"
        theDockerFile = 'Dockerfile'
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git([url: 'https://github.com/ranjitdas977/jenkins_cicd.git', branch:'main', credentialsId: 'jenkin_cicd'])
            }
        }
        stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build imagename
                }
            }
        }
        stage('Deploy Image'){
            steps{
                script {
                    docker.withRegistry('', registryCredential ) {
                        dockerImage.push("$BUILD_NUMBER")
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $imagename:$BUILD_NUMBER"
                sh "docker rmi $imagename:latest"
            }
        }
    }
}

pipeline {
    agent any
    //  environment{
    //     // IMAGE = "vladoz77/cicd-docker"
    //     // VERSION = "1.0.0"
        
    // }
    stages {
        stage('sync SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/vladoz77/cicd-docker-test'
            }
        }

        stage('build image') {
            steps {
                script {
                    dockerImage = docker.build("vladoz77/cicd-docker:${BUILD_NUMBER}") 
                }
            }
        }

        stage('image push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub') {
                        dockerImage.push ()
                        dockerImage.push('latest')
                    }
                }
            }
        }
       
    }
}
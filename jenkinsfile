pipeline {
    agent any

    stages {
        stage('sync SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/vladoz77/cicd-docker-test'
            }
        }

        stage('build image') {
            steps {
                script {
                    dockerImage = docker.build vladoz77/cicd-docker-test + ":$BUILD_NUMBER"
                }
            }
        }

        stage('image push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub') {
                        dockerImage.push()
                    }
                }
            }
        }
       
    }
}
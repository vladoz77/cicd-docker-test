pipeline {
    agent any
     environment{
        VERSION = "1.0.0"
        
    }
    stages {
        stage('sync SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/vladoz77/cicd-docker-test'
            }
        }

        stage('build image') {
            steps {
                script {
                  def app = docker.build("vladoz77/cicd-docker:${VERSION}-${BUILD_NUMBER}") 
                }
            }
        }

        stage('image push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub') {
                        app.push ()
                        app.push('latest')
                    }
                }
            }
        }
       
    }
}
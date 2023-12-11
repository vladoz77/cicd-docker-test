pipeline {
    agent any
     environment{
        IMAGE_NAME = "vladoz77/cicd-docker"
        VERSION = "1.0.0"
        IMAGE_TAG = "${VERSION}-${BUILD_NUMBER}"
        
    }

    stages {
        stage('sync SCM') {
            steps {
                checkout scm
            }
        }

        stage('build and push image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub'){
                        def app = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                        
                        app.inside {
                            sh 'echo "Tests passed"'
                            }
                        
                        app.push ()
                        app.push('latest')
                    }
                }
            }
        }

        stage('delete image') {
            steps {
                script{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"

                }
            }
        }
        
        stage("Checkout scm"){
            steps{
                build job: 'gitops-docker',  parameters: [credentials(name: 'IMAGE_TAG', value: "${IMAGE_TAG}")]
            }
        }
        
       
    }
}
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

        // stage('image push') {
        //     steps {
        //         script {
        //             withDockerRegistry(credentialsId: 'dockerhub') {
                        
        //             }
        //         }
        //     }
        // }
       
    }
}
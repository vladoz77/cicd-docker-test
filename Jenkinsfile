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

        stage('build image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub'){
                        def app = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                        
                        app.inside {
                            sh 'echo "Tests passed"'
                            }
                        
                        // app.push ()
                        // app.push('latest')
                    }
                }
            }
        }
        
        stage('Test-Image'){
            steps {
                script {
                    try {                           
                        def status = 0
                        status = sh(returnStdout: true, script: "container-structure-test test --image ${IMAGE_NAME}:${IMAGE_TAG} --config './app/unit-test.yaml' --json | jq .Fail") as Integer
                        if (status != 0) {                            
                            error 'Image Test has failed'
                        }

                    } catch (err) {
                        error "Test-Image ERROR: The execution of the container structure tests failed, see the log for details."
                        echo err
                    } 
                }
            }
        }

        
        stage('push image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub'){
                        docker.push ()
                        docker.push('latest')
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
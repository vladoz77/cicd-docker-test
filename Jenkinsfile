pipeline {
    agent any
     environment{
        VERSION = "1.0.0"
        
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
                        def app = docker.build("vladoz77/cicd-docker:{env.BUILD_NUMBER}")
                        
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
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

        stage('build and push image') {
            steps {
                    withDockerRegistry(credentialsId: 'dockerhub'){
                        def app = docker.build("vladoz77/cicd-docker:${VERSION}-${BUILD_NUMBER}")
                        app.push ()
                        app.push('latest')
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
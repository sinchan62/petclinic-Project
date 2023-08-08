pipeline {
    agent any
    stages {
        stage ('build'){
            steps {
                echo 'building docker image'
                sh "docker build -t petclinic-app ."
            }
        }
    }
    post {
        success {
            script {
                echo 'push image to docker hub'
                withCredentials([usernamePassword(credentialsId:"dockerhubcred", passwordVariable:"dockerhubpass", usernameVariable:"dockerhubuser")]) {
                    sh "docker tag petclinic-app ${env.dockerhubuser}/petclinic-app:latest"
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker push ${env.dockerhubuser}/petclinic-app:latest"
                }
            }
        }
        failure{
            script {
                echo 'skipping image push to dockerhub'
            }
        }
    }
}

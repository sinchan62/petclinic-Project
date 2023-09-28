pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        ECR_REPOSITORY = 'rohit-dummy-petclinic'
        DOCKER_IMAGE_TAG = "v${BUILD_NUMBER}"

        ECR_REGISTRY = "212392723633.dkr.ecr.ap-south-1.amazonaws.com"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building Docker image'
                sh "docker build -t rohit-dummy-petclinic ."
            }
        }

        stage('Push to AWS ECR') {
            steps {
                script {
                    def awsAccessKeyId = credentials('my-aws-credential')
                    def awsSecretAccessKey = credentials('my-aws-credential')

                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'my-aws-credential', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                        sh "aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $ECR_REGISTRY"
                        sh "docker tag rohit-dummy-petclinic:$DOCKER_IMAGE_TAG $ECR_REGISTRY/$ECR_REPOSITORY:$DOCKER_IMAGE_TAG"
                        sh "docker push $ECR_REGISTRY/$ECR_REPOSITORY:$DOCKER_IMAGE_TAG"
                    }
                }
            }
        }
    }
}

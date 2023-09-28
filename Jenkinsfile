pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'building docker image'
                sh "docker build -t rohit-dummy-petclinic ."
            }
        }

        stage('Test') {
            steps {
                echo 'manually.'
            }
        }
       stage('Push to AWS ECR') {
            steps {
                script {
                    def awsAccessKeyId = credentials('my-aws-credential')
                    def awsSecretAccessKey = credentials('my-aws-credential')
                    def awsRegion = 'ap-south-1'  // Replace with your desired AWS region
                    
                    def ecrRepository = 'rohit-dummy-petclinic'
                    def dockerImageTag = 'latest'  // Replace with the desired image tag
                    
                    sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 212392723633.dkr.ecr.ap-south-1.amazonaws.com"
                    sh "docker tag rohit-dummy-petclinic:latest 212392723633.dkr.ecr.ap-south-1.amazonaws.com/rohit-dummy-petclinic:latest"
                    sh "docker push 212392723633.dkr.ecr.ap-south-1.amazonaws.com/rohit-dummy-petclinic:latest"
                }
            }
        }
    }
}

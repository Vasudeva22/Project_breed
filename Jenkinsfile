pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = '116981809630'
        AWS_REGION = 'us-east-1'
        ECR_REPO_NAME = 'my_app_flask'
        DOCKER_IMAGE = "116981809630.dkr.ecr.us-east-1.amazonaws.com/my_app_flask"
    }

        stages {
            stage('Clone Repository') {
                steps{

                    //git url: 'https://github.com/Vasudeva22/Project_breed.git'
                    //git https://github.com/Vasudeva22/Project_breed.git
                    // Use git clone to clone the specified repository
                     sh 'git clone https://github.com/Vasudeva22/Project_breed.git'
                }
            }

            stage('Build Docker Image') {
                steps{
                    script {
                        //log in to Amazon ECR
                        sh'''
                        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 116981809630.dkr.ecr.us-east-1.amazonaws.com
                        '''

                        //Build Docker image
                        sh 'docker build -t my_app_flask:latest .'
                    }
                }
            }
            stage('Push Docker Image to ECR') {
                steps {
                    script {
                        //Push the docker image to ECR
                        sh 'docker push 116981809630.dkr.ecr.us-east-1.amazonaws.com/my_app_flask:latest'
                    }
                }
            }
            stage('Deploy to ECS') {
                steps {
                    script {
                        //Update ECS service to use new image 
                        sh '''
                          aws ecs update-service --cluster my_app_flask-cluster \
                                           --service my_app_flask-service \
                                           --force-new-deployment \
                                           --region us-east-1
                        '''
                    }
                }
            }
        }
}
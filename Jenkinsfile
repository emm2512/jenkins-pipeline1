pipeline {
    agent any 

    environment{
         AWS_REGION = 'us-east-1'
         ECR_REPO = '060795940509.dkr.ecr.us-east-1.amazonaws.com'
         IMAGE_ECR_REPO = '060795940509.dkr.ecr.us-east-1.amazonaws.com/jenkins-ci'
    }

    stages{
        stage('CodeScan'){
            steps{
                sh 'trivy fs . -o result.html'
                sh 'cat result.html'
            }
        }
        stage('dockerLogin'){
            steps{
               sh  "aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REPO"
            }
        }
        stage('dockerImageBuild'){
            steps{
                sh 'docker build -t jenkins-ci .'
                sh 'docker build -t imageversion .'
            }
        }
        stage('dockerImageTag'){
            steps{
                sh "docker tag jenkins-ci:latest\ |  $IMAGE_ECR_REPO:latest"
                sh "docker tag jenkins-ci:latest\ | $IMAGE_ECR_REPO:v1.$BUILD_NUMBER" 
            }

            }
        
        stage('pushImage'){
            steps{
                sh "$IMAGE_ECR_REPO:latest"
                sh "$IMAGE_ECR_REPO:v1.$BUILD_NUMBER"
            }
        }
    }
}
@Library("jenkins-functions") _
pipeline {
    agent { label 'ec2-agent' }
    environment {
        AWS_REGION   = "us-east-1"
        ECR_REGISTRY = "424369076274.dkr.ecr.us-east-1.amazonaws.com"
        IMAGE_NAME   = "django-notes-app"
        IMAGE_TAG    = "${BUILD_NUMBER}"
    }
    stages {
        stage("CODE") {
            steps {
                script {
                    clone("https://github.com/ashfaqbarkati786/django-notes-app.git", "main")
                }
            }
        }
        stage("Login to AWS ECR") {
            steps {
                script {
                    awsEcrLogin(env.AWS_REGION, env.ECR_REGISTRY, "aws-creds")
                }
            }
        }
        stage("Build Docker Image") {
            steps {
                script {
                    dockerBuild(env.IMAGE_NAME, env.IMAGE_TAG, env.ECR_REGISTRY)
                }
            }
        }
        stage("Push Docker Image to ECR") {
            steps {
                script {
                    dockerPush(env.IMAGE_NAME, env.IMAGE_TAG, env.ECR_REGISTRY)
                }
            }
        }
    }
}

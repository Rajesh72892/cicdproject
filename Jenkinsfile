pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        ACCOUNT_ID = '4752-5176-1877'   // Replace with your AWS Account ID
        REPO = 'rajesh-httpd'
    }

    stages {

        stage('Build') {
            steps {
                sh 'docker build -t rajesh-httpd .'
            }
        }

        stage('Login ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin \
                $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                '''
            }
        }

        stage('Tag') {
            steps {
                sh '''
                docker tag rajesh-httpd:latest \
                $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$REPO:latest
                '''
            }
        }

        stage('Push') {
            steps {
                sh '''
                docker push \
                $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$REPO:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'Image pushed to ECR successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}

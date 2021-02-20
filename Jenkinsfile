pipeline {
    agent any 
     environment {
        AWS_ACCESS_KEY_ID     = credentials('jenkins-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
        REGION                = "us-east-1"
        ECR_repo              = "456774515540.dkr.ecr.us-east-1.amazonaws.com/node"
        ECR_REPO_URL          = "456774515540.dkr.ecr.us-east-1.amazonaws.com"
    }      
    stages {
        stage('DOCKER IMAGE BUILD') {
            steps {
                sh '''
                    docker build -t $ECR_repo:node .
                    echo "completed"
                '''
            } 
        }
        stage('DOCKER IMAGE PUSH') {
            steps {
                sh '''
                    aws configure set region us-east-1
                    aws ecr get-login --region $REGION --no-include-email
                    # aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 456774515540.dkr.ecr.us-east-1.amazonaws.com
                    docker push $ECR_repo:node
                    echo "completed"
                '''
            } 
        }
         stage('Build') {
            steps {
                sh '''
                        aws eks --region us-east-1 update-kubeconfig --name demo
                        kubectl apply -f kube-deployment/deployment.yml
                   '''
            } 

        }

    }
}

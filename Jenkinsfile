pipeline {
    agent any 
     environment {
        AWS_ACCESS_KEY_ID     = credentials('jenkins-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
        REGION                = "us-east-1"
        ECR_REPO              = "456774515540.dkr.ecr.us-east-1.amazonaws.com/node"
        ECR_REPO_URL          = "456774515540.dkr.ecr.us-east-1.amazonaws.com"
        EKS_CLUTER_NAME       = "demo"
 
    }      
    stages {
        stage('DOCKER IMAGE BUILD') {
            steps {
                sh '''
                    docker build -t $ECR_REPO:node .
                    echo "completed"
                '''
            } 
        }
        stage('DOCKER IMAGE PUSH') {
            steps {
                sh '''
                    aws configure set region $REGION
                    $(aws ecr get-login --region $REGION --no-include-email)
                    docker push $ECR_REPO:node
                    docker logout $ECR_REPO_URL
                    echo "completed"
                '''
            } 
        }
         stage('Build') {
            steps {
                sh '''
                        aws eks --region $REGION update-kubeconfig --name $EKS_CLUTER_NAME
                        kubectl apply -f kube-deployment/deployment.yml
                   '''
            } 

        }

    }
}

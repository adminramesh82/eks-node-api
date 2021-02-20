pipeline {
    agent any 
     environment {
        AWS_ACCESS_KEY_ID     = credentials('jenkins-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
        REGION                = "eu-central-1"
        ECR_REPO              = "234934568007.dkr.ecr.eu-central-1.amazonaws.com/ecr_repo"
        ECR_REPO_URL          = "234934568007.dkr.ecr.eu-central-1.amazonaws.com"
        EKS_CLUTER_NAME       = "eks_cluster_tuto"
 
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
                    #$(aws ecr get-login --region $REGION --no-include-email)
                    #aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin 234934568007.dkr.ecr.eu-central-1.amazonaws.com
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
                        /usr/local/bin/kubectl apply -f kube-deployment/deployment.yml
                   '''
            } 

        }

    }
}

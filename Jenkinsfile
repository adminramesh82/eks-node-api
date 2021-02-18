pipeline {
    agent any 
     environment {
        AWS_ACCESS_KEY_ID     = credentials('jenkins-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
        REGION                = 'us-east-1'
    }      
    stages {
        stage('DOCKER IMAGE BUILD') {
            steps {
                sh '''
                    docker build -t 456774515540.dkr.ecr.us-east-1.amazonaws.com/node:node .
                    echo "completed"
                '''
            } 
        }
        stage('DOCKER IMAGE PUSH') {
            steps {
                sh '''
                    aws configure set region $REGION
                    $(aws ecr get-login --region $REGION --no-include-email)
                    docker push 456774515540.dkr.ecr.us-east-1.amazonaws.com/node:node
                    echo "completed"
                '''
            } 
        }
        stage('Build') {
            steps {
                kubernetesDeploy(
                    configs: 'kube-deployment/deployment.yml',
                    kubeconfigId: 'kube8s',
                    enableConfigSubstitution: true
                )  
            } 

        }
    }
}

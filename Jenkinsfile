pipeline {
    agent any 
     environment {
        AWS_ACCESS_KEY_ID     = credentials('jenkins-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
        REGION                = 'eu-cental-1'
    }      
    stages {
        stage('DOCKER IMAGE BUILD') {
            steps {
                sh '''
                    docker build -t 234934568007.dkr.ecr.eu-central-1.amazonaws.com/ecr_repo:node .
                    echo "completed"
                '''
            } 
        }
        stage('DOCKER IMAGE PUSH') {
            steps {
                sh '''
                    # aws configure set region $REGION
                    # aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin 234934568007.dkr.ecr.eu-central-1.amazonaws.com
                    docker push 234934568007.dkr.ecr.eu-central-1.amazonaws.com/ecr_repo:node
                    echo "completed"
                '''
            } 
        }
         stage('Build') {
            steps {
        sh '''
                pwd
                KUBECONFIG=/var/lib/jenkins/.kube/config /usr/local/bin/kubectl get all
                # /usr/local/bin/kubectl apply -f kube-deployment/deployment.yml
           '''
            } 

        }

    }
}

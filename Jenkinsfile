pipeline {
    agent any

    environment {
        SSH_PRIVATE_KEY = credentials('cca56d5a-1f7a-4911-8238-3b917ef90ddc')
    }

    stages {
        stage('Build productive') {
            when {
                branch 'main'
            }
            steps {
                sh 'mkdir .ssh'
                sh 'echo $SSH_PRIVATE_KEY > .ssh/id_rsa'
                sh 'ssh master01.k8s.thejay.azubi.server.lan'
                // Checkout/Update git repo
                sh 'git clone git@github.com:the-jay-team/deployments.git'
                sh 'cd deployments'
                sh 'git pull'

                // Prod
                sh 'kubectl apply -f jaytube-prod/namespace.yaml'

                // Stage
                sh 'kubectl apply -f jaytube-stage/namespace.yaml'

                // Pipeline
                sh 'kubectl apply -f pipeline/namespace.yaml'
                sh 'kubectl apply -f pipeline/jenkins-serviceaccount.yaml'
                sh 'kubectl apply -f pipeline/jenkins-deployment.yaml'
                sh 'kubectl apply -f pipeline/jenkins-service.yaml'

                // Vault
                sh 'kubectl apply -f vault/namespace.yaml'
            }
        }
    }
}
pipeline {
    agent any
    stages{
        //stage for clone mi private rpository
        stage('Clone Repo'){
            steps{
                git branch: 'master', credentialsId: 'gitlab', url: 'https://gitlab.com/clibercastillo/school-app.git'
            }
        }
        stage('Docker Build and Push'){
            steps{
                sh '''
                    docker build -t clibercastillo/school-app:${BUILD_NUMBER} .
                    docker login -u clibercastillo -p $DOCKER_PASSWORD
                    docker push clibercastillo/school-app:${BUILD_NUMBER}
                '''
            }
        }
        stage('Scan Container'){
            steps{
                sh '''
                    docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy clibercastillo/school-app:${BUILD_NUMBER}
                '''
            }
        }
        //create stage for push to ECR
        stage('Push to ECR'){
            steps{
                sh '''
                    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789.dkr.ecr.us-east-1.amazonaws.com
                    docker tag clibercastillo/school-app:${BUILD_NUMBER} 123456789.dkr.ecr.us-east-1.amazonaws.com/school-app:${BUILD_NUMBER}
                    docker push 123456789.dkr.ecr.us-east-1.amazonaws.com/school-app:${BUILD_NUMBER}
                '''
            }
        }
        stage('Deploy to Kubernetes'){
            steps{
                sh '''
                    kubectl apply -f kubernetes/deployment.yaml
                    kubectl apply -f kubernetes/service.yaml
                '''
            }
        }
    }
}







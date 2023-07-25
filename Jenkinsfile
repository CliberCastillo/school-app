pipeline {
    agent any
    stages{
        stage('Verify Branch'){
            steps{
                echo "$GIT_BRANCH"
            }
        }
        stage('Docker Build'){
            steps{
                sh 'docker images -a'
                sh '''
                    docker build -t clibercastillo/school-app:${BUILD_NUMBER} .
                    docker images -a
                '''
            }
        }
    }
}
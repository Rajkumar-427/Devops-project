pipeline {
    agent any
    environment {
        REGISTRY = "rajkumar427/cicd_django"
        VERSION = "${env.BUILD_ID}"
        PREVIOUS_VERSION = VERSION - 1
    }
    stages {
        stage('Database Update') {
            steps {
                sh 'docker cp django-container:/app/database/db.sqlite3 ./database || true'
            }
        }
        stage('Building image') {
            steps{
                sh 'docker build -t ${REGISTRY}:${VERSION} .'
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD docker.io'
                    sh 'docker push ${REGISTRY}:${VERSION}'
                }
            }
        }
        stage('Removing the old container'){
            steps {
                sh 'docker rm -f django-container'
                sh 'docker rmi -f ${REGISTRY}:${PREVIOUS_VERSION}'
                sh 'docker images'
                sh 'docker ps'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker pull ${REGISTRY}:${VERSION}'
                sh 'docker run -d -p 8000:8000 --name django-container ${REGISTRY}:${VERSION}'
                sh 'docker ps'
            }
        }
        
    }
}

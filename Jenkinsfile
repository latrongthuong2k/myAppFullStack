pipeline {

    agent any

    tools { 
        maven 'my-docker'
    }
    environment {
        MYSQL_ROOT_LOGIN = credentials('mysql-root-login')
    }
    stages {

        stage('pull image') {
            steps {
               withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/')
               docker.image(thuong191020/my-repository:v1.0.1).pull()
            }
        }
        stage('Clean and Deploy') {
            steps {
                sh 'docker-compose -f compose.yaml down --remove-orphans'
                sh 'docker-compose -f compose.yaml up -d'
            }
        }
 
    }
    post {
        // Clean after build
        always {
            cleanWs()
        }
    }
}
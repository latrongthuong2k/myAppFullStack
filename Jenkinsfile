pipeline {

    agent any

    tools { 
        maven 'my-maven' 
    }
    environment {
        MYSQL_ROOT_LOGIN = credentials('mysql-root-login')
    }
    stages {

        // stage('Build with Maven') {
        //     steps {
        //         sh 'mvn --version'
        //         sh 'java -version'
        //         sh 'mvn clean package -Dmaven.test.failure.ignore=true'
        //     }
        // }

        stage('Packaging/Pushing imagae') {

            steps {
                withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/') {
                    sh 'docker build -t thuong191020/my-repository:v1.0.1 .'
                    sh 'docker push thuong191020/my-repository:v1.0.1'
                }
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
pipeline {
    agent any
    tools {
        maven 'MAVEN3'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/tengaleh/maven-web-app.git'
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [
                    tomcat11(
                        credentialsId: 'tomcat-creds',
                        path: '',
                        url: 'http://<TOMCAT-IP>:8080'
                    )
                ],
                contextPath: 'maven-web-app',
                war: 'target/maven-web-app.war'
            }
        }
    }
    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}

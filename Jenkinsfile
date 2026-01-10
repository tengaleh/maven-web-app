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
                sh '''
                curl -u tomcat:tomcat123 \
                -T target/maven-web-app.war \
                http://54.226.145.246:8080/manager/text/deploy?path=/maven-web-app&update=true
                '''
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

pipeline {
    agent {
        label 'slave-node-1'
    }
    tools {
        maven 'MAVEN3'
    }
    environment {
        TOMCAT_HOME = "/opt/tomcat"
        APP_NAME = "maven-web-app"
    }
    stages {
        stage('Checkout Source Code') {
            steps {
                echo "Code checked out from SCM automatically"
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Stop Tomcat') {
            steps {
                sh '''
                $TOMCAT_HOME/bin/shutdown.sh || true
                '''
            }
        }
        stage('Deploy WAR to Tomcat') {
         steps {
                sh '''
                rm -rf $TOMCAT_HOME/webapps/$APP_NAME*
                cp target/$APP_NAME.war $TOMCAT_HOME/webapps/
                '''
            }
        }
        stage('Start Tomcat') {
            steps {
                sh '''
                $TOMCAT_HOME/bin/startup.sh || true
                '''
            }
        }
    }
    post {
        success {
            echo "Application deployed successfully using Jenkinsfile from SCM"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}

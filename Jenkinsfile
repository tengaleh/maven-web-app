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
                echo 'Deploying the application to Tomcat...'
                script {
                    def warFile = "${APP_NAME}.war"
                    // Copy the generated .war file from the Jenkins workspace to the Tomcat webapps directory
                    sshPut remote: "${TOMCAT_HOME}/webapps/${warFile}",
                            server: '52.90.133.114', // Replace with your Tomcat server IP
                            credentials: 'slave-ssh-key', // The ID from Jenkins Credentials Manager
                            from: "target/${warFile}" // Location of the .war file after the Maven build
                }
                echo 'Deployment successful. Application should be available at http://<tomcat-server-ip>:8080/my-webapp'
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

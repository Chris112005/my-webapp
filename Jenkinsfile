pipeline {
    agent any

    environment {
        MVN_HOME = tool 'Maven'  // Make sure Maven is set up in Jenkins Global Tools
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Chris112005/my-webapp.git'
            }
        }

        stage('Build') {
            steps {
                bat "\"%MVN_HOME%\\bin\\mvn\" clean install"
            }
        }

        stage('Test') {
            steps {
                bat "\"%MVN_HOME%\\bin\\mvn\" test"
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'tomcat-creds', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASS')]) {
                    bat '''
                        setlocal EnableDelayedExpansion
                        curl -u !TOMCAT_USER!:!TOMCAT_PASS! -T target\\my-webapp.war "http://localhost:8090/manager/deploy?path=/my-webapp&update=true"
                        endlocal
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Build Failed!"
        }
    }
}

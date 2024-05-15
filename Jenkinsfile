pipeline {
    agent any

    environment {
        NODE_HOME = tool name: 'Soumya'
        PATH = "${NODE_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Soumya795/angular-jenkin.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Install Angular CLI') {
            steps {
                bat 'npm install -g @angular/cli'
                bat 'set PATH=%PATH%;%APPDATA%\\npm'
            }
        }

        stage('Build') {
            steps {
                bat '''
                set PATH=%PATH%;%APPDATA%\\npm
                ng build --configuration production
                '''
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def tomcatPath = "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps\\ROOT"

                    bat """
                        del /Q /S "${tomcatPath}\\*"
                        xcopy /E /I /Y dist\\angular-jenkin\\* "${tomcatPath}"
                    """
                }
            }
        }

        stage('Restart Tomcat') {
            steps {
                bat 'net stop Tomcat9'
                bat 'net start Tomcat9'
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

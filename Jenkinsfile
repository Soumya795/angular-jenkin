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
                    def tomcatUser = 'admin'
                    def tomcatPassword = 'admin'
                    def tomcatHost = 'localhost'
                    def tomcatPort = '8081'
                    def tomcatWebapp = 'ROOT'

                    bat """
                        del /Q /S \\path\\to\\tomcat\\webapps\\${tomcatWebapp}\\*
                        xcopy /E /I /Y dist\\angular-jenkin\\* \\path\\to\\tomcat\\webapps\\${tomcatWebapp}
                    """
                }
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

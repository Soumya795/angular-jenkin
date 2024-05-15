pipeline {
    agent any

    environment {
        NODE_HOME = tool name: 'Soumya'
        PATH = "${NODE_HOME}/bin:${env.PATH}"
        CATALINA_HOME = 'C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0'
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
                    def tomcatPath = "${env.CATALINA_HOME}\\webapps\\ROOT"

                    bat """
                        if not exist "${tomcatPath}" mkdir "${tomcatPath}"
                        del /Q /S "${tomcatPath}\\*"
                        xcopy /E /I /Y dist\\angular-jenkin\\* "${tomcatPath}"
                    """
                }
            }
        }

        stage('Restart Tomcat') {
            steps {
                script {
                    bat """
                        set CATALINA_HOME=${env.CATALINA_HOME}
                        cd "${env.CATALINA_HOME}\\bin"
                        startup.bat
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

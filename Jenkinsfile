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
                bat 'set PATH=%PATH%;%NODE_HOME%\\node_modules\\.bin;%APPDATA%\\npm'
            }
        }

        stage('Build') {
            steps {
                script {
                    def nodeBinPath = bat(script: 'echo %NODE_HOME%\\node_modules\\.bin', returnStdout: true).trim()
                    def npmBinPath = bat(script: 'echo %APPDATA%\\npm', returnStdout: true).trim()
                    bat "set PATH=%PATH%;${nodeBinPath};${npmBinPath}"
                    bat 'ng build --prod'
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def tomcatUser = 'your-tomcat-username'
                    def tomcatPassword = 'your-tomcat-password'
                    def tomcatHost = 'your-tomcat-host'
                    def tomcatPort = '8080'
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

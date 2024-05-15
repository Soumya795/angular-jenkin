pipeline {
    agent any

    environment {
        NODE_HOME = tool name: 'Soumya'
        NPM_GLOBAL = bat(script: 'npm root -g', returnStdout: true).trim()
        PATH = "${NODE_HOME}/bin:${env.PATH};${NPM_GLOBAL}/.bin"
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
            }
        }

        stage('Build') {
            steps {
                bat 'ng build --prod'
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

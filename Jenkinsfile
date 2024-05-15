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
                    def tomcatPath = 'C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps\\ROOT'

                    // Check if the Tomcat directory exists
                    bat 'if not exist "' + tomcatPath + '" mkdir "' + tomcatPath + '"'

                    // Clear the existing ROOT directory
                    bat 'del /Q /S "' + tomcatPath + '\\*"'

                    // Copy the new build files to the ROOT directory
                    bat 'xcopy /E /I /Y dist\\angular-jenkin\\* "' + tomcatPath + '"'

                    // Optionally, restart Tomcat (ensure you have the right permissions)
                    bat '"C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\bin\\shutdown.bat"'
                    bat '"C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\bin\\startup.bat"'
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

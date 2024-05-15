pipeline {
    agent any

    environment {
        NODE_HOME = tool name: 'NodeJS'
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
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'ng build --prod'
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

                    sh """
                        sshpass -p ${tomcatPassword} ssh ${tomcatUser}@${tomcatHost} 'rm -rf /path/to/tomcat/webapps/${tomcatWebapp}/*'
                        sshpass -p ${tomcatPassword} scp -r dist/angular-jenkin/* ${tomcatUser}@${tomcatHost}:/path/to/tomcat/webapps/${tomcatWebapp}/
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

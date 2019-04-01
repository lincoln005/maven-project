pipeline {
    agent any
    stages{
        stage('Build'){
            steps{
                bat 'mvn clean package'
                bat "docker build C:\Program Files\Docker Toolbox -t tomcatwebapp:${env.BUILD_ID}"
            }
        }
    }
}
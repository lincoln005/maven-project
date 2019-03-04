pipeline {
    agent any

tools {
        maven 'localMaven'
        jdk 'localJDK'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '3.86.147.119', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '3.87.5.244', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                      sh "scp -i ~\.ssh\ec2.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Prod"){
                    steps {
                        sh "scp -i ~\.ssh\ec2.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
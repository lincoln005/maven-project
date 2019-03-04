pipeline {
    agent any

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
                      sh "echo y | plink -ssh -i /C:/Users/I7Dell/.ssh/ec2.pem ec2-user@3.86.147.119 "exit" plink -ssh ec2-user@3.86.147.119 sh "ls"""
                    }
                }

                stage ("Deploy to Prod"){
                    steps {
                      sh "echo y | plink -ssh -i /C:/Users/I7Dell/.ssh/ec2.pem ec2-user@3.86.147.119" "exit" plink -ssh ec2-user@3.87.5.244 sh "ls"""
                    }
                }
            }
        }
    }
}
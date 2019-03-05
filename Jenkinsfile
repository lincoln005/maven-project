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
                      sh "pscp -B -vvv -i /C:/Users/I7Dell/.ssh/ec2.pem ec2-user@3.86.147.119 /C:/Users/I7Dell/projects/maven-project/webapp/src/main/webapp/webtest.txt  ec2-user@3.86.147.119:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Prod"){
                    steps {
                      sh "pscp -B -vvv -i scp -i /C:/Users/I7Dell/.ssh/ec2.pem ec2-user@3.87.5.244 /C:/Users/I7Dell/projects/maven-project/webapp/src/main/webapp/webtest.txt  ec2-user@3.87.5.244:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
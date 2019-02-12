pipeline {
    agent any
	
	tools {
        maven 'localMaven'
        jdk 'localJDK'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '3.82.92.83', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '3.82.246.59', description: 'Production Server')
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
                    echo 'Now Archiving psckage...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i ~/.ssh/tomcat-key.pem -o StrictHostKeyChecking=no **/target/webapp/*.* ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i ~/.ssh/tomcat-key.pem -o StrictHostKeyChecking=no **/target/webapp/*.* ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}
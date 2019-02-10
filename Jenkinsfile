pipeline {
    agent any
	
	tools {
        maven 'localMaven'
        jdk 'localJDK'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '3.94.180.147', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '3.87.211.173', description: 'Production Server')
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
                        sh "cp -i C:/'SSH Keys'/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i C:/'SSH Keys'/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
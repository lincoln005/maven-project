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
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                      sh "scp -i ~/.ssh/tomcat-key.pem C:/Users/I7Dell/projects/maven-project/webapp/target/webapp.war ec2-user@3.82.246.59:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i C:/Users/I7Dell/.ssh/tomcat-key.pem C:/Users/I7Dell/projects/maven-project/webapp/target/webapp.war ec2-user@3.82.92.83:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
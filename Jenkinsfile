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
                       echo 'message deploy Now dev Archiving...'
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        echo 'message deploy Now prod Archiving...'
                    }
                }
            }
        }
    }
}
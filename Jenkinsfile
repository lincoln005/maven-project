pipeline {

    agent any

      parameters {

         string(name: 'tomcat_dev', defaultValue: '3.86.147.119 ', description: 'Staging Server')

         string(name: 'tomcat_prod', defaultValue: '3.87.5.244', description: 'Production Server')

         string(name: 'ppk_path', defaultValue: 'C:/Users/I7Dell/.ssh/ec2.ppk', description: 'full path to ppk file')

         string(name: 'war_path', defaultValue:"C:/Users/I7Dell/projects/maven-project/webapp/src/main/webapp/webtest.txt", description: 'full path to war file')

         string(name: 'target_path', defaultValue:'/var/lib/tomcat/webapps/', description: 'full path in the target server')

    }



    triggers {

         pollSCM('* * * * *')

     }



stages{

        stage('Build'){

            steps {

                bat 'mvn clean package'

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

                        bat "echo y | pscp -i ${ppk_path} ${war_path} ec2-user@${params.tomcat_dev}:${target_path}"

                    }

                }



                stage ("Deploy to Production"){

                    steps {

                        bat "echo y | pscp -i ${ppk_path} ${war_path} ec2-user@${params.tomcat_prod}:${target_path}"

                    }

                }

            }

        }

    }

}
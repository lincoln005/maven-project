pipeline {

    agent any

      parameters {

         string(name: 'tomcat_dev', defaultValue: '18.233.154.198', description: 'Staging Server')

         string(name: 'tomcat_prod', defaultValue: '3.82.150.153', description: 'Production Server')

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

                         sh "scp -B -vvv -i /C:/Users/I7Dell/.ssh/ec2.pem ec2-user@18.233.154.198 /C:/Users/I7Dell/projects/maven-project/webapp/src/main/webapp/webtest.txt  ec2-user@18.233.154.198:/var/lib/tomcat7/webapps"

                    }

                }



                stage ("Deploy to Production"){

                    steps {

                        sh "scp -B -vvv -i scp -i /C:/Users/I7Dell/.ssh/ec2.pem ec2-user@3.82.150.153 /C:/Users/I7Dell/projects/maven-project/webapp/src/main/webapp/webtest.txt  ec2-user@3.82.150.153:/var/lib/tomcat7/webapps"

                    }

                }

            }

        }

    }

}
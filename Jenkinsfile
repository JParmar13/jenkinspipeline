pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '35.176.220.190', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '35.178.23.43', description: 'Production Server')
    } 

    tools {
        maven 'localMaven'
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
                        bat "echo y | scp -i C:/Users/Jay/Documents/DevOps/jenkins.pem C:/Program Files (x86)/Jenkins/workspace/Fullyautomated/**/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "echo y | scp -i C:/Users/Jay/Documents/DevOps/jenkins.pem C:/Program Files (x86)/Jenkins/workspace/Fullyautomated**/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
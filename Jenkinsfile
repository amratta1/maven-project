pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '192.168.1.60', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '192.168.1.61', description: 'Production Server')
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
                        sh "scp  **/target/*.war root@${params.tomcat_dev}:/var/lib/tomcat9/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh " **/target/*.war root@${params.tomcat_prod}:/var/lib/tomcat9/webapps"
                    }
                }
            }
        }
    }
}

pipeline {
    agent any
	tools {
    maven 'MAVEN_HOME'
  }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '34.207.224.81', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.207.224.81', description: 'Production Server')
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
                        sh "scp **/target/*.war jenkins@${params.tomcat_dev}:/opt/apache-tomcat-8.5.38-staging/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp **/target/*.war jenkins@${params.tomcat_prod}:/opt/apache-tomcat-8.5.38-production/webapps"
                    }
                }
            }
        }
    }
}

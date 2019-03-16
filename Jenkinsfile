pipeline {
    agent any
	tools {
    maven 'MAVEN_HOME'
  }

    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
				sh 'docker build . -t tomcatwebapp:${env.BUILD_ID}'
            }
            
        }
	}
}

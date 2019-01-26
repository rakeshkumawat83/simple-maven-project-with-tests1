pipeline { 
    agent any 
	   def mvnHome
	def JAVA1
	tools {
        	mvnHome 'M3'
    	}
	
    stages {
	stage('Preparation') {
            steps {
                git 'https://github.com/rakeshkumawat83/simple-maven-project-with-tests.git'
            }
        }

        stage('Build') { 
            steps { 
                bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
        stage('Test'){
            steps {
                junit '**/target/surefire-reports/TEST-*.xml'
     		archive 'target/*.jar'
            }
        }
    }
}


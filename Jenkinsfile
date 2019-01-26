pipeline { 
    agent any 

tools {
  maven 'M3'
  jdk 'JAVA'
}

environment {
  JAVA_HOME = "JAVA"
  MAVEN_HOME = "M3"
}
	
    stages {
	stage('Preparation') {
            steps {
                git 'https://github.com/rakeshkumawat83/simple-maven-project-with-tests.git'
            }
        }

        stage('Build') { 
            steps { 
                bat(/"mvn" -Dmaven.test.failure.ignore clean package/)
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

pipeline { 
    agent any 

tools {
  maven 'M3'
  jdk 'JAVA'
}

environment {
  JAVA_HOME = "JAVA"
  MAVEN_HOME = "M3"
  SONAR_URL = "http://localhost:9000"
  BUILD_LOCATION = "BuildLocation";
}
	
    stages {
	stage('Preparation') {
            steps {
                git 'https://github.com/rakeshkumawat83/simple-maven-project-with-tests.git'
            }
        }

        stage('Build') { 
            steps { 
                bat "mvn -Dmaven.test.failure.ignore clean package"
            }
        }
        stage('Test'){
            steps {
                junit '**/target/surefire-reports/TEST-*.xml'
     		archive 'target/*.jar'
            }
        }
	    
	stage('Sonarqube analysis') {
    	    steps {
              script {
                 scannerHome = tool 'SonarScanner';
              }
          withSonarQubeEnv('SonarQube') {
		  bat "${scannerHome}/bin/sonar-runner.bat -e -Dsonar.host.url=${SONAR_URL} -Dsonar.projectName=SimpleMavenProject -Dsonar.sources=. -Dsonar.projectKey=SimpleMavenProject:SimpleMavenProject -Dsonar.java.binaries=${BuildLocation}/TFS_Pipeline_Project/target/test-classes/test" 
             }
          }
        }
	    
	stage ("SonarQube Quality Gate") {
             steps {   
                script {
                    def qualitygate = waitForQualityGate()
                    if (qualitygate.status != "OK") {
                        error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
                    }
                }
             }
	}	
    }
}

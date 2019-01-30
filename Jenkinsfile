/* Declarative pipeline must be enclosed within a pipeline block */
pipeline { 
    // agent section specifies where the entire Pipeline will execute in the Jenkins environment
    agent any 
    /**
     * tools to auto-install and put on the PATH
     * some of the supported tools - maven, jdk, gradle
     */
tools {
  maven 'M3'
  jdk 'JAVA'
}

 /**
     * environment provides variables set in env variables or local variables
     */
environment {
  JAVA_HOME = "JAVA"
  MAVEN_HOME = "M3"
  SONAR_URL = "http://localhost:9000"
  BUILD_LOCATION = "BuildLocation";
}

 /**
     * stages contain one or more stage directives
     */	
    stages {
        // Checkout Git reporitory 
        stage('Checkout Git') {
            steps {
                   git 'https://github.com/rakeshkumawat83/simple-maven-project-with-tests.git'
            }
        }

        // Build reporitory using Maven tool
        stage('Build') { 
            steps { 
                bat "mvn -Dmaven.test.failure.ignore clean package"
            }
        }

        // Unit Test using Junit and archive results for analysis
        stage('Unit Test and Archive Results'){
            steps {
                junit '**/target/surefire-reports/TEST-*.xml'
     		    archive 'target/*.jar'
            }
        }

        // Automation Testing using selenium or SOAPUI
        stage('Automation Test'){
            parallel {
                //Automation Testing using Firefox browser
                stage('Automated Test Firefox'){
                    steps {
                    bat 'echo Automation Testing using Firefox browser...'
                    }
                }
                //Automation Testing using Chrome browser
                stage('Automated Test Chrome'){
                    steps {
                    bat 'echo Automation Testing using Chrome browser...'
                    }
                }
            }
            
        }

        // Code Coverage analysis using Sonarqube scanner	            
	    stage('Code Coverage analysis with Sonarqube') {
    	    steps {
                script {
                 scannerHome = tool 'SonarScanner';
                }
            withSonarQubeEnv('SonarQube') {
		        bat "${scannerHome}/bin/sonar-runner.bat -e -Dsonar.host.url=${SONAR_URL} -Dsonar.projectName=SimpleMavenProject -Dsonar.sources=. -Dsonar.projectKey=SimpleMavenProject:SimpleMavenProject -Dsonar.java.binaries=${BuildLocation}/TFS_Pipeline_Project/target/test-classes/test" 
                }
            }
        }

        // SonarQube Quality Gate check	    
	    stage ("SonarQube Quality Gate") {
            steps {   
                /*script {
                    def qualitygate = waitForQualityGate()
                    if (qualitygate.status != "OK") {
                        error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
                    }
                }*/
                bat 'echo SonarQube Quality Gate check...'
            }
	    }	

        // Artifacts Repository using Artifactory
        stage('Artifacts Repository'){
            steps {
                bat 'echo Artifacts Repository using Artifactory...'
            }
        }

        // Security Scanning (dynamic analysis using Burp Suite)
        stage('Security Scanning'){
            steps {
                bat 'echo Security Scanning (dynamic analysis using Burp Suite)...'
            }
        }

        // Deploy code using Configuration Manager
        stage('Deploy'){
            steps {
                bat 'echo Deploy code using Configuration Manager...'
            }
        }


    }
}

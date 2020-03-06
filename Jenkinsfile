pipeline {
  agent {label 'BuildSecurityNode1'}
  
  tools {
    maven 'Maven'
  }
  
  stages {
    
    stage ('SAST SECRETS') {
      steps {
	sh 'rm -rf output || true'
	sh 'mkdir output || true'
        sh 'rm trufflehog | true'
        sh 'docker run gesellix/trufflehog --json https://github.com/cmarcond/WebGoat-1.git | jq "." > trufflehog'
        sh 'cat trufflehog > output/SAST-secrets.json'
      }
    }
	  
    stage ('SAST OWASP') {
      steps {
        sh 'rm owasp-dependency-check.sh* || true'
        sh 'cp /tmp/owasp-dependency-check.sh .'
        sh 'chmod +x owasp-dependency-check.sh'
        sh 'bash owasp-dependency-check.sh'
	    sh 'mv /opt/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.html output/SAST-dependency-check.html'
	      
      }
    }
	  
    stage ('SAST SONAQUBE') {
      steps {
        withSonarQubeEnv('sonarqube') {
          sh 'mvn clean package sonar:sonar'
        }
      }
    }

    stage('BUILD') {
      steps {
        sh 'mvn clean package'
      }
    }
}

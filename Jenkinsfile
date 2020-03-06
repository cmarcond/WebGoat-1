pipeline {
  agent {label 'Security'}
  
  tools {
    maven 'Maven'
  }
  
  stages {
    
/*    stage ('SAST SECRETS') {
      steps {
	sh 'rm -rf output || true'
	sh 'mkdir output || true'
        sh 'rm trufflehog | true'
        sh 'docker run gesellix/trufflehog --json https://github.com/cmarcond/WebGoat-1.git | jq "." > trufflehog'
        sh 'cat trufflehog > output/SAST-secrets.json'
      }
    }
*/	  
    stage ('SAST OWASP') {
      steps {
	sh 'rm -rf output || true'
	sh 'mkdir output || true'
	sh ' /opt/jenkins/dependency-check/bin/dependency-check.sh --proxyserver ncproxy1 --proxyport 8080 --project Testing --out . --scan .'
	sh 'mv dependency-check-report.html output/SAST-dependency-check.html'      
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
	
}

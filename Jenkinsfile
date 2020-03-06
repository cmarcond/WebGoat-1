pipeline {
  agent {label 'Security'}
  
  tools {
    maven 'Maven'
  }
  
  stages {
    
    stage ('SAST SECRETS') {
      steps {
	sh 'rm -rf output || true'
	sh 'mkdir output || true'
        sh 'rm trufflehog | true'
        sh 'trufflehog --regex --entropy=False https://github.com/mshauneu/trufflehog  > output/SAST-secrets.txt'
      }
    }
	  
    stage ('SAST OWASP') {
      steps {
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

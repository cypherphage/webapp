pipeline {
  agent any
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
          echo "PATH=${PATH}"
          echo "M2_HOME=${M2_HOME}"
          echo "hello"
        '''
      }
    }
    
    stage ('Check-git-secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/cypherphage/webapp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    
    stage('Source Composition Analysis') {
      steps {
        sh 'rm owasp* || true'
        sh 'wget https://raw.githubusercontent.com/cypherphage/webapp/master/owasp-dependency-check.sh'
        sh 'chmod +x owasp-dependency-check.sh'
        sh 'bash owasp-dependency-check.sh'
      }
    }
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }      
    } 
    
    stage ('Deploy to tomcat') {
      steps {
        sshagent(['tomcat']) {
          sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@18.206.246.23:/home/ubuntu/prod/apache-tomcat-9.0.52/webapps/webapp.war'
        }
        
      }
    }
   
  }
}

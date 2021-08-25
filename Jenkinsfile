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

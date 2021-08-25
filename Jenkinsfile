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

pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
          echo "PATH = ${PATH}"
          echo "M2_HOME = ${M2_HOME}"
        ''' 
      }
    }
    
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage ('Deploy-To-Tomcat') {
      steps {
        sshagent(['tomcat']) {
          // Upload the WAR file to a temporary directory
          sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@172.31.14.151:/tmp/webapp.war'

          // Use SSH to move the WAR file into the Tomcat directory with sudo
          sh '''
            ssh ubuntu@172.31.14.151 "sudo mv /tmp/webapp.war /opt/tomcat/webapps/webapp.war"
          '''
        }      
      }       
    }
  }  
}

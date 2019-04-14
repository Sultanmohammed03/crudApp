pipeline {
  agent any
  tools {
        maven 'maven2'
    }
  stages {
    stage ('checkout code') {
      steps {
        git 'https://github.com/Sultanmohammed03/crudApp'
      }
   }
    stage ('Run Build') {
      steps {
        sh "/opt/apache-maven-3.3.9/bin/mvn clean package"
      }
    }
    stage ('Nexus') {
      steps {
      nexusArtifactUploader artifacts: [[artifactId: 'crudApp', classifier: '', file: 'target/crudApp.war', type: 'war']], credentialsId: 'nexus', groupId: 'Central', nexusUrl: '10.0.0.43:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'releases', version: '2.$(BUILD_NUMBER)'
      }
    }
    stage ('Copy to tomcat') {
      steps {
        sh "sudo wget http://10.0.0.43:8081/nexus/service/local/repositories/releases/content/Central/crudApp/2.$(BUILD_NUMBER)/crudApp-2.$(BUILD_NUMBER).war -O /opt/apache-tomcat-9.0.17/webapps/crudApp.war"
        sh "sudo sh /opt/apache-tomcat-9.0.17/bin/catalina.sh stop"
        sh "sudo sh /opt/apache-tomcat-9.0.17/bin/catalina.sh start"
      }
    }
  }
}

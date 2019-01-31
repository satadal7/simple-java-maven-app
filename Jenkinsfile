pipeline {
  agent any

tools {
  maven 'maven3.6.0'
  jdk 'java1.8.0'
  }
stages {
  stage('Build'){
  steps {
    sh "mvn -B -DskipTests clean package"
    }
  }
  stage('Test'){
    steps {
      sh "mvn test"
    }
    post {
      always{
        junit 'target/surefire-reports/*.xml'
      }
    }
  }
  stage('Deploy'){
    steps{
     // sh 'java -jar target/my-app-1.0-SNAPSHOT.jar'
     sh 'scp -o StrictHostKeyChecking=no -i /MARS_POC.pem target/my-app-1.0-SNAPSHOT.jar centos@34.221.84.5:/tmp/'
     sh 'ssh -i /MARS_POC.pem centos@34.221.84.5 java -jar /tmp/my-app-1.0-SNAPSHOT.jar'
    }
  }
  }
}

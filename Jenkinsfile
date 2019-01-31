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
      sh 'java -jar target/my-app-1.0-SNAPSHOT.jar'
      scp -o StrictHostKeyChecking=no -i /tmp/my.pem target/my-app-1.0-SNAPSHOT.jar centos@54.149.242.5:/tmp/
      ssh -i /tmp/MARS_POC.pem centos@54.149.242.5 java -jar /tmp/my-app-1.0-SNAPSHOT.jar
    }
  }
  }
}

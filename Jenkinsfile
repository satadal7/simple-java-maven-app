pipeline {
  agent any

tool {
  maven 'maven3.6.0'
  jdk 'java1.8.0'
  }
stages {
  stage('Build'){
  steps {
    sh "mvn -B -DskipTests clean package"
    }
  }
  }
}

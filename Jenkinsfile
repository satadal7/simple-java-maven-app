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
  
  stage('clone'){
        git branch:'master',url:"https://github.com/satadal7/simple-java-maven-app.git"
        }
  stage('Artifactory configuration') {
    steps{
      rtServer{
        id:'ARTIFACTORY_SERVER',
        url:'http://34.216.127.228:8081/artifactory',
        credentialsId:'artifactoryCre'
       }
      
      rtMavenDeployer{
        id:"MAVEN_DEVELOPER",
        serverId:"ARTIFACTORY_SERVER",
        reseaseId:"libs-release-local",
        snapshotRepo:"libs-snapshot-local"
      }
        rtMavenResolver{
        id:"MAVEN_RESOLVER",
        serverId:"ARTIFACTORY_SERVER",
        reseaseId:"libs-release",
        snapshotRepo:"libs-snapshot"
      }
    }
  }
  stage('Exec Maven'){
    steps{
      rtMavenRun{
        tool:'maven3.6.0',
          pom:'pom.xml',
          goals:'clean install',
          deployerId:"MAVEN_DEVELOPER",
          resolverId:"MAVEN_RESOLVER"
      }
    }
  }
  stage('publish biuld info'){
    steps{
      rtpublishBuildInfo{
        serverId:"ARTIFACTORY_SERVER"
      }
    }
    stage('Deploy'){
    steps{
     // sh 'java -jar target/my-app-1.0-SNAPSHOT.jar'
     sh 'scp -o StrictHostKeyChecking=no -i /tmp/MARS_POC.pem target/my-app-1.0-SNAPSHOT.jar centos@34.221.84.5:/tmp/'
     sh 'ssh -i /tmp/MARS_POC.pem centos@34.221.84.5 java -jar /tmp/my-app-1.0-SNAPSHOT.jar'
    }
  }
  }
  }
  
  }
}

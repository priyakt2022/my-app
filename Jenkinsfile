@Library("app-lib@main") _
pipeline {
  agent any
  tools {
    maven 'maven3'
  }
  options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '10', numToKeepStr: '7')
  }
  parameters {
    choice choices: ['branch name', 'dev', 'test', 'prod'], description: 'choose the branch to build', name: 'choose choice'
  }
  stages {
    stage("git checkout") {
      steps {
        git branch: 'master', url: 'https://github.com/priyakt2022/my-app'
      }
    }
    stage("Maven build") {
      steps {
        sh 'mvn clean package'
      }
    }
    stage("Deploy to Tomcat") {
      steps {
        sshagent(['tomcat-dev']) {
            
          tomcatDeploy(["13.234.136.143","13.234.136.143","13.234.136.143"],"ec2-user","tomcat-dev")    
        }
      }
    }
  }
  post {
    success {
      archiveArtifacts artifacts: 'target/*.war'
      cleanWs()
    }
  }
}

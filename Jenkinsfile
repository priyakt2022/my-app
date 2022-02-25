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
          //copy war file to tomcat
          sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.33.131:/opt/tomcat8/webapps/app.war"
          sh "ssh ec2-user@172.31.33.131 /opt/tomcat8/bin/shutdown.sh"
          sh "ssh ec2-user@172.31.33.131 /opt/tomcat8/bin/startup.sh"
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

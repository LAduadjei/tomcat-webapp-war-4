
node {
  
  def mavenHome = tool name: "maven3"
  
  properties([ pipelineTriggers([pollSCM('* * * *')]) ])
  
  stage('1. SCM - clone code') { 
      echo "Cloning the code now" // the comment will show up in the console
      git branch: 'Scripted', changelog: false, poll: false, url: 'https://github.com/LAduadjei/tomcat-webapp-war-4'
  }
  
  stage('2 Maven - build code') { 
      echo "Building code now"
      sh "${mavenHome}/bin/mvn clean package"
  }
  
  stage('3 Sonar - CodeQuality') { 
      echo "Running quality scan"
      sh "${mavenHome}/bin/mvn sonar:sonar"
  }
  
  stage('4 Nexus - Artifactory') { 
      echo "to artifactory"
      sh "${mavenHome}/bin/mvn deploy"
  }
  
  // cd = cont. deployment. (automatic deployments)
  // cd = cont. delivery ((Production)) approval before deployment
  stage('5. Approval') {
      echo "Please provide approvel"
      timeout(time: 5, unit: 'DAYS') {
         input message: "Please approve deployment"
      }
  }
  
  stage('6 Deploy- tomcat') {
       echo 'deploy 2 tomcat'
       deploy adapters: [tomcat9(credentialsId: '88252b84-e85f-486e-a277-9a375e0f3df9', path: '', url: 'http://3.239.198.189:8080/')], contextPath: '/Scripted', war: 'target/*war'
  }
} 

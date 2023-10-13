node{

def mavenHome = tool name : "Maven3.9.4" 
	properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * * ')])])

stage('CheckOutCode'){

git branch: 'development', credentialsId: '4ec04451-03eb-448f-b075-02ec4c463385', url: 'https://github.com/lavanyasep/maven-web-application.git'

}

stage('Build'){

sh "${mavenHome}/bin/mvn clean package"
}
stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}
stage('UploadArtifactIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}
stage('DeployApplicationIntoTomcatServer'){
sshagent(['45ff30f0-7de2-4c99-b44d-51f2073a8bf2']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.109.201.130:/opt/apache-tomcat-9.0.80/webapps"
	
}
}
}//node closing

node{
def mavenHOME = tool name: 'maven3.9.9', type: 'maven'
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '3'))])
stage ('CheckOutCode') {
git branch: 'development', credentialsId: '948de7bd-e500-478d-bd8b-a14fd9865ba4', url: 'https://github.com/narentechnologies-bank/maven-web-application.git'
}
stage ('Build') {
sh "${mavenHOME}/bin/mvn clean package"
}
stage ('ExecuteSonarQubeReport') {
sh "${mavenHOME}/bin/mvn clean package sonar:sonar"
}
stage ('UploadArtifactsIntoNexus') {
sh "${mavenHOME}/bin/mvn clean package sonar:sonar deploy"
}
stage ('DeployAppIntoTomcatServer') {
sshagent(['af76b1a4-2ab3-43c7-8fa3-ecc93be55adb']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.12.129:/opt/apache-tomcat-9.0.98/webapps/"
}//closing sshagent
}//closing stage
}//node closing

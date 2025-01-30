node{
def mavenHOME = tool name: 'maven3.9.9', type: 'maven'
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '3'))])
stage ('CheckOutCode'){
git branch: 'development', credentialsId: '61e40864-2209-4b34-b9b4-0a17dbf498c2', url: 'https://github.com/narentechnologies-bank/maven-web-application.git'
}
stage ('Build'){
sh "${mavenHOME}/bin/mvn clean package"
}
stage ('ExecuteSonarQubeReport'){
sh "${mavenHOME}/bin/mvn clean sonar:sonar"
}
stage ('UploadArtfactsIntoNexus'){
sh "${mavenHOME}/bin/mvn clean deploy"
}
stage ('DeployAppIntoTomcatServer'){
sshagent(['46873769-85fa-4569-ad9a-be0982a937d6']){
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.9.159:/opt/apache-tomcat-9.0.98/webapps/"

}
}//stage closing

}//node closing

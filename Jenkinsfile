node
{
def mavenHome = tool name: "maven3.6.3"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '2', daysToKeepStr: '', numToKeepStr: '3')), pipelineTriggers([pollSCM('* * * * *')])])

stage('Checkoutcode')
{
git branch: 'development', credentialsId: '95b33bf0-abf9-48eb-a968-d66c558d1a01', url: 'https://github.com/apssdcvinay/maven-web-application.git'
}
stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
stage('sonar')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage('nexus')
{
sh "${mavenHome}/bin/mvn deploy"
}
stage('tomcat')
{
sshagent(['c5068c04-38e4-4888-aee4-109f41ba7b35']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@54.225.43.155:/opt/apache-tomcat-9.0.50/webapps/"
}
}
stage('mail')
{
emailext body: '''Regards 
K Vinay''', subject: 'Build over', to: 'karnatakavinay6@gmail.com'
}
}

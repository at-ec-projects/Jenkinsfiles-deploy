node
{
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
    def mavenHome = tool name: "maven3.6.3"
    stage('CheckoutCode')
    {
        git branch: 'development', credentialsId: '8ec5c4ca-15f7-4a06-9da1-92a10160eb65', url: 'https://github.com/at-ec-projects/riders-application.git'
    }
    stage('Build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('ExecuteSonarQubeReport')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('UploadArtifactsToNexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('DeployApplicationOnTomcat')
    {
        sshagent(['cbc0fe80-f7f7-4e0d-b9bb-017949febfd0']) 
        {
            sh "scp -o StrictHostKeyChecking=no target/riders-application.war ec2-user@3.7.55.245:/opt/apache-tomcat-9.0.37/webapps/"
}
    }
    stage('SendEmailNotification')
    {
        emailext attachLog: true, body: '''Regards
Abhishek
+918818877113''', subject: 'Build is over', to: 'abhishektiwari30@gmail.com'
    }
}

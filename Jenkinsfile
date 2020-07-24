node
{
    //Node level declarations
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
    def mavenHome = tool name: "maven3.6.3"
    //Getting the code from Github
    stage('CheckoutCode')
    {
        git branch: 'development', credentialsId: '8ec5c4ca-15f7-4a06-9da1-92a10160eb65', url: 'https://github.com/at-ec-projects/riders-application.git'
    }
    //Creation of package
    stage('Build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    //Execution of SonarQube Reports
    stage('ExecuteSonarQubeReport')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    //Uploading of Artifacts into the Nexus Artifactory
    stage('UploadArtifactsToNexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
    //Deployment of Application on Tomcat Server
    stage('DeployApplicationOnTomcat')
    {
        sshagent(['cbc0fe80-f7f7-4e0d-b9bb-017949febfd0']) 
        {
            sh "scp -o StrictHostKeyChecking=no target/riders-application.war ec2-user@3.7.55.245:/opt/apache-tomcat-9.0.37/webapps/"
}
    }
    //Sending Email to Devops Engineers and Developers
    stage('SendEmailNotification')
    {
        emailext attachLog: true, body: '''Regards
Abhishek
+918818877113''', subject: 'Build is over', to: 'abhishektiwari30@gmail.com'
    }
}

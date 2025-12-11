pipeline
{
  agent any
  
  tools
  {
    maven 'Maven'
  }
  
  triggers
  {
    pollSCM('* * * * *')
  }
  
  options
  {
    timestamps()
    buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
  }
  
  stages
  {
    stage('Checkout Code from GitHub')
    {
      steps()
      {
        git branch: 'master', credentialsId: '3712815b-bbd7-40df-a932-c13923458418', url: 'https://github.com/sanepallinagendra/maven-web-application.git'
      }
    }
    
    stage('Build Project')
    {
      steps()
      {
        sh "mvn clean package"
      }
    }
    
    stage('Execute SonarQube Report')
    {
      steps()
      {
        sh "mvn clean sonar:sonar"
      }
    }
    
    stage('Upload Artifacts to Sonatype Nexus')
    {
      steps()
      {
        sh "mvn clean deploy"
      }
    }
    
    stage('Deploy Application to Tomcat')
    {
      steps()
      {
        sshagent(['bfe1b3c1-c29b-4a4d-b97a-c068b7748cd0'])
        {
          sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.154.190.162:/opt/apache-tomcat-9.0.50/webapps/"
        }
      }
    }
  }

post
{
  success
  {
    emailext to: 'devopstrainingblr@gmail.com,mithuntechnologies@yahoo.com',
    subject: "Pipeline Build is Over Build # is ${env.BUILD_NUMBER} and Build Status is ${currentBuild.result}",
    body: "Pipeline Build is Over Build # is ${env.BUILD_NUMBER} and Build Status is ${currentBuild.result}",
    replyTo: 'devopstrainingblr@gmail.com'
  }
  failure
  {
    emailext to: 'devopstrainingblr@gmail.com,mithuntechnologies@yahoo.com',
    subject: "Pipeline Build is Over Build # is ${env.BUILD_NUMBER} and Build Status is ${currentBuild.result}",
    body: "Pipeline Build is Over Build # is ${env.BUILD_NUMBER} and Build Status is ${currentBuild.result}",
    replyTo: 'devopstrainingblr@gmail.com'
    }
  }
}

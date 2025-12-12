pipeline
{
    agent any
    tools
    {
       maven "Maven-3.9.8"
    }
	stages
	{
	    stage('git checkout')
         {
         steps{
                git branch: 'dev', url: 'https://github.com/Neerajsinghmakhan245/maven-webapplication-project-kkfunda.git'
             }
         }


         stage('Compile')
         {
         steps
         {
          sh "mvn compile"
         }

         }
         stage('Build')
         {
         steps
         {
              sh "mvn clean package"
         }
         }
         stage('SQ Report')
         {
         steps 
         {

         	sh "mvn sonar:sonar"
         }

         }
         stage('Deploy to nexus')
           {
              steps
              {
                sh "mvn clean deploy"
              }
           }
           stage('Deploy to tomcat')
           {
              steps
              {
                 sh """

      curl -u kk:password \
--upload-file /var/lib/jenkins/workspace/Jio-Declaritive-pipeline/target/maven-web-application.war \
"http://13.204.219.24:8080/manager/text/deploy?path=/maven-web-application&update=true"
          
        """
              }
           }
	}//End stages
	post {
  success {

    script
    {
     notifyBuild(currentBuild.result)
    }
    
  }
  failure {

  script
  {
    notifyBuild(currentBuild.result)

  }
   
  }
}

}//End pipeline

def notifyBuild(String buildStatus = 'STARTED') {
    buildStatus = buildStatus ?: 'SUCCESS'

    def colorCode
    def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
    def summary = "${subject} (${env.BUILD_URL})"

    switch (buildStatus) {
        case 'STARTED':
            colorCode = '#FFFF00' // Yellow
            break
        case 'SUCCESS':
            colorCode = '#00FF00' // Green
            break
        default:
            colorCode = '#FF0000' // Red
    }

    slackSend(color: colorCode, message: summary)
}

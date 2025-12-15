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
                git branch: 'qa', url: 'https://github.com/Neerajsinghmakhan245/maven-webapplication-project-kkfunda.git'
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
		stage('bsnl-UAT'){
	steps
	{
		build job:'bsnl-UAT'
	}
}
		
	}//End stages
	
}//End pipeline

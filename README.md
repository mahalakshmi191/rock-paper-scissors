Step-by-step Jenkins Tomcat deploy of a WAR file
A] Jenkins Tomcat deploy prerequisites
The following pieces of software are required to follow this example of a Jenkins deployment of a WAR file to Tomcat:
•	 Java web application that Jenkins can deploy to Tomcat;
•	 Git source code management tool installation;
•	 Java 8 or newer installation of the JDK;
•	 build tool -- this tutorial uses Maven, but any build tool, such as Ivy, ANT or Gradle, that can package a Java application into a WAR file will do; and
•	 Jenkins CI tool installation.
Web application is needed to deploy on tomcat so used: free to clone my rock-paper-scissors Java web app from GitHub:
git clone https://github.com/cameronmcnz/rock-paper-scissors.git

2 Ways Deployment was done:
✅** 1. Deployed rps.war using Maven (Manually)**
	You ran Maven commands like: mvn clean install
	This generated a .war file (likely rps.war or roshambo.war) inside the target/ directory.
	You may have tested it locally by manually deploying to Tomcat via:
•	Copying to Tomcat webapps/ folder
•	Or accessing http://localhost:8081/<context-path>
•	  
•	PS C:\Users\Admin\java-web_apps\rock-paper-scissors> mvn clean install tomcat7:deploy
•	[INFO] Scanning for projects...
•	[INFO]
•	[INFO] ---------------------< com.mcnz.rps.web:roshambo >----------------------
•	[INFO] Building roshambo web app 






**2. Deployed roshambo.war using Jenkins CI/CD Pipeline**
1.	Add a user with WAR deployment rights to the tomcat-users.xml.
2.	Add the Deploy to container Jenkins Tomcat plugin.
3.	Create a Jenkins build job with a Deploy to container post-build action.
4.	Run the Jenkins build job.
5.	Test to ensure the Jenkins deployment of the WAR file to Tomcat was successful.
6.	Successfully able to hit 8081 in which tomcat is configured

  

 
 
git checkout patch-1
Pay special attention to the last command, where you will switch to the patch-1 branch. The master branch creates a JAR file, with Tomcat embedded within. But what we need is the patch-1 branch, which creates a WAR file that can be deployed to Tomcat by Jenkins.

 

 
 




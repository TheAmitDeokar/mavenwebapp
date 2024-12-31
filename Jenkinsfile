node{
	def mavenHome = tool name: "maven3.9.9"

  	 // Checkout stage
   	stage('CheckoutCode'){
   	git branch: 'development ', credentialsId: '1358129b-d0cb-468d-b528-6919a0509bb8', url: 'https://github.com/TheAmitDeokar/mavenwebapp.git.git'
    	 }

   	// Build stage
   	stage('Build'){
	sh "$mavenHome/bin/mvn clean package"
         }

        // Generate SonarQube Report
        stage('GenerateSonarQubeReport'){
        sh "$mavenHome/bin/mvn sonar:sonar"
        }

	// Upload artifact into Artifactory repository 
        stage('UploadSonarQubeReport'){
        sh "$mavenHome/bin/mvn deploy"
        }
   
       // Deploy application into Tomcat server
	stage('DeployAppIntoTomcat'){
        sshagent(['9c342b89-95b4-43c0-a167-b09179544e5d']) {
          sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.235.79.106:/opt/apache-tomcat-9.0.98/webapps"
         }
        }
   
    } // node closing

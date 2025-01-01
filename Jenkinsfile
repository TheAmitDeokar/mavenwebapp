node{
	def mavenHome = tool name: "maven3.9.9"
 
	echo "The NODE_NAME is : " ${env.NODE_NAME}

	echo "The JOB_NAME is : " ${env.JOB_NAME}

	echo "The BUILD_NUMBER is : " ${env.BUILD_NUMBER}
  	 // Checkout stage
   	stage('CheckoutCode'){
   	git branch: 'development ', credentialsId: '1358129b-d0cb-468d-b528-6919a0509bb8', url: 'https://github.com/TheAmitDeokar/mavenwebapp.git'
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
          sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.206.127.254:/opt/apache-tomcat-9.0.98/webapps"
         }
        }
   
    } // node closing

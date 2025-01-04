node{
	def mavenHome = tool name: "maven3.9.9"

	buildName 'Dev -  ${BUILD_NUMBER}'  // Changes buil name like from 20 to Dev - 20
	buildDescription 'Pipeline Script - Scriptedway' // will add des about build
	echo "The NODE_NAME is :  ${env.NODE_NAME} "

	echo "The JOB_NAME is :  ${env.JOB_NAME} "

	echo "The BUILD_NUMBER is :  ${env.BUILD_NUMBER} "

	// To keep last 5 buil only, old one will be delete
        properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5', removeLastBuild: true)), pipelineTriggers([])])
	// Github will notify to jenkins once changes/commit is done in github and jenkins starts build automatically 
//	properties([pipelineTriggers([githubPush()])])
  	 
	// Checkout stage
   	stage('CheckoutCode'){ 
   	git branch: 'development ', credentialsId: '1358129b-d0cb-468d-b528-6919a0509bb8', url: 'https://github.com/TheAmitDeokar/mavenwebapp.git'
    	 }

   	// Build stagee
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
          sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@52.66.243.190:/opt/apache-tomcat-9.0.98/webapps"
         }
        }
   
    } // node closing

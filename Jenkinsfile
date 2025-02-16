
node{
    def mavenHome = tool name: "maven3.9.9"
    def buildNumber = BUILD_NUMBER

// To keep last 5 buil only, old one will be delete
        properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5', removeLastBuild: true)), pipelineTriggers([])])

// Github will notify to jenkins once changes/commit is done in github and jenkins starts build automatically 
       properties([pipelineTriggers([githubPush()])])

	
    // Checkout stage
    stage('Git Clone'){
        git branch: 'development', credentialsId: 'GithubCred', url: 'https://github.com/TheAmitDeokar/mavenwebapp.git'
    }
    
    // Build stagee
   	stage('Build app package'){
	sh "$mavenHome/bin/mvn clean package"
         }
      
     // Build Docker Image
    stage('Build Docker image'){
    sh "docker build -t 686255940829.dkr.ecr.ap-south-1.amazonaws.com/maven-web-application:${buildNumber} ."
    } 
    
    // Docker Login and push image
    stage('Docker Login and push image'){
    
   sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 686255940829.dkr.ecr.ap-south-1.amazonaws.com"       

   sh "docker push 686255940829.dkr.ecr.ap-south-1.amazonaws.com/maven-web-application:${buildNumber}"
    }
    
    // Deploy App as Docker Container in Docker Deployment server
     stage('Deploy App as Docker Container in Docker Deployment server'){
        sshagent(['ubuntudeploymentserver']) {

          sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.43.211 docker rm -f mavenwebappcontainer || true"

          sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 686255940829.dkr.ecr.ap-south-1.amazonaws.com" 

         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.43.211 docker run -d --name mavenwebappcontainer -p 8081:8080 686255940829.dkr.ecr.ap-south-1.amazonaws.com/maven-web-application:10"  
    }
     }

}
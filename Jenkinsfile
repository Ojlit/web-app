node {
	
	def MHD = tool 'M3'

	stage('1.CodeClone'){
	git credentialsId: 'a89e31ce-2b35-4318-9f9d-f1c25ee53708', url: 'https://github.com/Ojlit/web-app'
	}
	stage('2.bulildArtifact'){
	sh "${MHD}/bin/mvn package"
	}
	stage('3.CodeQuality'){
	sh "${MHD}/bin/mvn sonar:sonar"
	}
	stage('4.Upload2Nexus'){
	sh "${MHD}/bin/mvn deploy"
	}
	stage('5.Deploy2Stage'){
	sshagent(['32d5fb4f-d92f-4a10-9f12-2738eab55fcc']) {
   sh "scp -o StrictHostKeyChecking=no target/*war ec2-user@172.31.15.31:/opt/tomcat9/webapps/app"
	}
	stage('6.Appproval2Proceed'){
	    timeout(time:5, unit:'DAYS'){
 			input message: 'Approval for production'
	}
	stage('7.Deploy2Prod'){
	sshagent(['32d5fb4f-d92f-4a10-9f12-2738eab55fcc']) {
   sh "scp -o StrictHostKeyChecking=no target/*war ec2-user@172.31.15.31:/opt/tomcat9/webapps/app.war"
	}
	stage('8.EmailN'){
	emailext body: '''Hello Scrum Team,

I thought to inform you of the progress we have made with deploying release 13 of the new software.''', recipientProviders: [developers()], subject: 'CiCd Status', to: 'Scrum Team'
	}
	
	}
	
}
		
	}
  

node('master') {
	stage ('checkout code'){
		checkout scm
	}
	
	stage ('Build'){
		shell "mvn clean install -Dmaven.test.skip=true"
	}

	stage ('Test Cases Execution'){
		shell "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}

	stage ('Sonar Analysis'){
		shell 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9005'
	}

	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
	}
	
	stage ('Deployment'){
		shell 'cp target/*.war /C:/Users/Apache Software Foundation/apache-tomcat-8.5.58/webapps'
	}
	stage ('Notification'){
		slackSend color: 'good', message: 'Deployment Sucessful'
		emailext (
		      subject: "Job Completed",
		      body: "Jenkins Pipeline Job for Maven Build got completed !!!",
		      to: "sudeepthi_chakram@hcl.com"
		    )
	}
}

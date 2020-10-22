node('master') {
	stage ('checkout code'){
		checkout scm
	}
	
	stage ('Compile'){
		shell "mvn clean install -Dmaven.test.skip=true"
	} 
	
	stage ('Test Cases Execution'){
		shell "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}
	
	stage ('Sonar Analysis'){
		shell 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9005'
	}

	stage ('Build'){
		shell "mvn clean install package -Dmaven.test.skip=true"
	}
	
	stage ('Archive Artifacts'){
		//junit '**/target/surefire-reports/TEST-*.xml'
      //archiveArtifacts 'target/*.jar'
		//archiveArtifacts(artifacts: 'target/*.war', fingerprint: true) 
		shell "mvn insall tomcat7:deploy"
		//deploy adapters: [tomcat7(credentialsId: '8217496f-3b5c-4dab-9bdf-e30380271946', path: '', url: 'http://192.168.0.106:8086')], contextPath: 'rps', war: '*/*.war'
	}
	
	stage ('Deployment'){
		shell 'cp target/*.war /C:/Users/Apache Software Foundation/apache-tomcat-8.5.58/webapps'
	}
	stage ('Notification'){
		//slackSend color: 'good', message: 'Deployment Sucessful'
		emailext (
		      subject: "Job Completed",
		      body: "Jenkins Pipeline Job for Maven Build got completed !!!",
		      to: "sudeepthi516@gmail.com"
		    )
	}
}

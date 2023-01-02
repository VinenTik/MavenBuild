node('') {
	stage ('checkout code'){
		checkout scm
	}
	
	stage ('Build'){
		sh "mvn clean install -Dmaven.test.skip=true"
	}

	stage ('Test Cases Execution'){
		sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}

	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
	}
	stage ('Notification'){
		emailext attachLog: true, attachmentsPattern: 'target/surefire-reports/*.xml', 
			 body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
	Check console output at $BUILD_URL to view the results.''', 
			compressLog: true, recipientProviders: [buildUser(), requestor()], subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'tikoovinay7@gmail.com'
	}
}

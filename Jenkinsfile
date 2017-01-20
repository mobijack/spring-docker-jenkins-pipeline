#!groovy
node('master') {

	// Default docker registry is docker hub
	registryUrl = "docker.io"
    
	if (getBinding().hasVariable("CONTAINER_REGISTRY"))
	{
		registryUrl=$CONTAINER_REGISTRY
	}
	println "  The docker registryUrl = $registryUrl"

	stage('Checkout') {
	
		echo 'Starting checkout'

		checkout poll: false, 
		scm: [$class: 'GitSCM', branches: [[name: '*/master']], 
		doGenerateSubmoduleConfigurations: false, 
		extensions: [[$class: 'CleanBeforeCheckout']], 
		submoduleCfg: [], 
		userRemoteConfigs: [[url: 'http://ns162002@almgit.ncr.com/scm/~ns162002/spring-docker-jenkins-pipeline.git']]]

		echo 'Checkout complete'
	}	

	stage('Build') {

		echo 'Starting build'
		
		// Run the maven build
		if (isUnix()) {
			echo 'build - TODO'
		} else {
			bat(/mvnw.cmd package -Ddocker.registry=$registryUrl/)
		}
		
		echo 'build complete'		
	} 

	stage('Integration Test') {
        echo 'Starting Integration Test'
		
		if (isUnix()) {
			echo 'Verify - TODO'
		} else {
			bat(/mvnw verify/)
		}
		
        echo 'Integration Test complete'
	}

	stage ('Push to Registry?') {
		timeout(time: 1, unit: 'HOURS') {
			input 'Deploy to Repository?'
		}
	}
	
	stage('Push Image') {
	
		// Run the maven build
		if (isUnix()) {
			echo 'Push Image - TODO'
		} else {
			bat(/mvnw docker:push/)
		}
	}	
} 

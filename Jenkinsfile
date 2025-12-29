pipeline{
	agent any
	environment {
		REGISTRY = "docker.io/fethibny"
		IMAGE = "demoapp"
		TAG = "v1.${BUILD_NUMBER}"
	}
	stages {
		stage('Build') {
			steps {
			sh "docker build -t $REGISTRY/$IMAGE:$TAG ."
				}
		}
		
		stage('Test'){
			steps{
		sh "docker run --rm -d -p 3000:3000 $REGISTRY/$IMAGE:$TAG"
		sh "sleep 2 && curl -fsSL http://localhost:3000"
		}
}	
		stage('Push'){
			steps{
				withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
		sh "echo $PASS | docker login -u $USER --password-stdin"
		sh "docker push $REGISTRY/$IMAGE:$TAG"
}	
	}
	}
		stage('Deploy') {
			steps { 
				sshagent(['deploy-server-key']) {
					sh "ssh user@server 'docker pull $REGISTRY/$IMAGE:$TAG && docker run -d -p 8080:300 $REGISTRY/$IMAGE:$TAG'"
				}
			}
		}
}



}

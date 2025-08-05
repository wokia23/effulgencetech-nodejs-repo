
pipeline{

	agent any

	//rename the user name michaelgwei86 with the username of your dockerhub repo
	environment {
		DOCKERHUB_CREDENTIALS=credentials('DOCKERHUB_CREDENTIALS')
		IMAGE_REPO_NAME = "wokia23/effulgencetech-nodejs-image"
		CONTAINER_NAME= "effulgencetech-nodejs-cont-"
	}
	
//Downloading files into repo
	stages {
		stage('Git checkout') {
            		steps {
                		echo 'Cloning project codebase...'
                		git branch: 'main', url: 'https://github.com/Michaelgwei86/effulgencetech-nodejs-repo.git'
            		}
        	}
	
//Building and tagging our Docker image

		stage('Build-Image') {
			
			steps {

				//Building image for Dockerhub repo

				//sh 'docker build -t wokia23/effulgencetech-nodejs-image:$BUILD_NUMBER .'
				sh 'docker system prune -f'
				sh 'docker container prune -f'
				sh 'docker build -t $IMAGE_REPO_NAME:$BUILD_NUMBER .'
				sh 'docker images'
			}
		}
		
//Logging into Dockerhub
		stage('Login to Dockerhub') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

//Building and tagging our Docker container
		stage('Build-Container') {


				//sh 'docker run --name effulgencetech-node-cont-$BUILD_NUMBER -p 8082:8080 -d wokia23/effulgencetech-nodejs-image:$BUILD_NUMBER'
				sh 'docker run --name $CONTAINER_NAME-$BUILD_NUMBER -p 8085:8080 -d $IMAGE_REPO_NAME:$BUILD_NUMBER'
				sh 'docker ps'
			}
		}

//Pushing the image to the docker

		stage('Push to Dockerhub') {
			//Pushing image to dockerhub
			steps {
				//sh 'docker push wokia23/effulgencetech-nodejs-image:$BUILD_NUMBER'
				sh 'docker push $IMAGE_REPO_NAME:$BUILD_NUMBER'
			}
		}
        
	}


post {
    always {
        script {
            def COLOR_MAP = [
                'SUCCESS': 'good', 
                'FAILURE': 'danger',
            ]
            slackSend channel: '#devops', color: COLOR_MAP[currentBuild.currentResult], message: "*${currentBuild.currentResult}:*, Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
        echo 'I will always say Hello again!'
    }
}
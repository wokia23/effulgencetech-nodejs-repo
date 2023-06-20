
def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
]

pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('DOCKERHUB_CREDENTIALS')
		IMAGE_REPO_NAME = "michaelgwei86/effulgencetech-nodejs-img"
		CONTAINER_NAME= "effulgencetech-nodejs-cont-"
	}

	stages {

		stage('Build-Image') {
			//Building and tagging our Docker image
			//rename the user name michaelgwei86 with the username of your dockerhub repo
			steps {
				//Building image for Dockerhub repo

				//sh 'docker build -t michaelgwei86/effulgencetech-nodejs-image:$BUILD_NUMBER .'
				sh 'docker build -t $IMAGE_REPO_NAME:$BUILD_NUMBER .'
				sh 'docker images'
			}
		}

		stage('Login to Dockerhub') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Build-Container') {
			//Building and tagging our Docker container
			//rename the user name michaelgwei86 with the username of your dockerhub repo
			steps {

				//sh 'docker run --name effulgencetech-node-cont-$BUILD_NUMBER -p 8082:8080 -d michaelgwei86/effulgencetech-nodejs-image:$BUILD_NUMBER'
				sh 'docker run --name $CONTAINER_NAME-$BUILD_NUMBER -p 8087:8080 -d $IMAGE_REPO_NAME:$BUILD_NUMBER'
				sh 'docker ps'
			}
		}


		stage('Push to Dockerhub') {
			//Pushing image to dockerhub
			steps {
				//sh 'docker push michaelgwei86/effulgencetech-nodejs-image:$BUILD_NUMBER'
				sh 'docker push wokia23/effulgencetech-nodejs-image/$IMAGE_REPO_NAME:$BUILD_NUMBER'
			}
		}
        
	}

    post { 
        always { 
            echo 'I will always say Hello again!'
            slackSend channel: '#effulgencetech-devops-channel', color: COLOR_MAP[currentBuild.currentResult], message: "*${currentBuild.currentResult}:*, Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
    }

}
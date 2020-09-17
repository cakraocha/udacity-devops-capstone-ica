pipeline {
    agent any
    stages {

        stage('Lint HTML') {
            steps {
                sh 'tidy -q -e *.html'
            }
        }

        stage('Build Docker Image') {
			steps {
                /* Be sure to grant permission using sudo chmod 666 /var/run/docker.sock */
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-hub-ica', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
						docker build -t cakraocha/udacity-capstone-ica .
					'''
				}
			}
		}

        stage('Push Image to Dockerhub') {
			steps {
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-hub-ica', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
						docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
						docker push cakraocha/udacity-capstone-ica
					'''
				}
			}
		}

        stage('Set current kubectl context') {
			steps {
				withAWS(region:'us-east-1', credentials:'ica-devops-capstone') {
					sh '''
						kubectl config use-context arn:aws:eks:us-east-1:558180201505:cluster/capstone-ica
					'''
				}
			}
		}
        
    }
}
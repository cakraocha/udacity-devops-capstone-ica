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

        stage('Set config AWS EKS') {
            steps {
                withAWS(region:'us-east-1', credentials:'ica-devops-capstone') {
                    sh '''
                        aws eks --region us-east-1 update-kubeconfig --name capstone-ica
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

        stage('Deploy blue container') {
			steps {
				withAWS(region:'us-east-1', credentials:'ica-devops-capstone') {
					sh '''
						kubectl apply -f ./blue-controller.json
					'''
				}
			}
		}

        stage('Deploy green container') {
			steps {
				withAWS(region:'us-east-1', credentials:'ica-devops-capstone') {
					sh '''
						kubectl apply -f ./green-controller.json
					'''
				}
			}
		}

        stage('Create the service in the cluster, redirect to blue') {
			steps {
				withAWS(region:'us-east-1', credentials:'ica-devops-capstone') {
					sh '''
						kubectl apply -f ./blue-service.json
					'''
				}
			}
		}

        stage('Wait user approval') {
            steps {
                input "Ready to redirect traffic to green?"
            }
        }

        stage('Create the service in the cluster, redirect to green') {
			steps {
				withAWS(region:'us-east-1', credentials:'ica-devops-capstone') {
					sh '''
						kubectl apply -f ./green-service.json
					'''
				}
			}
		}

        stage('Check and run Kubernetes deployment') {
			steps {
				withAWS(region:'us-east-1', credentials:'ica-devops-capstone') {
					sh '''
						kubectl get services \
                        kubectl get pods \
                        kubectl describe services bluegreenlb
					'''
				}
			}
		}
        
    }
}
pipeline {
environment {
	registry = "gopinathvs/mydockerrep"
	registryCredential = 'dockerhub'
	dockerhub=''
	tagName = "capstoneblue"
}
agent any
    stages {
        stage('Lint HTML') {
            steps {
                sh 'tidy -q -e *.html'
            }
        }

    	stage('Building image') {
		steps{
        		script {
          			dockerImage = docker.build registry + ":" + tagName
        		}
      		}
    	}

	stage('Deploy Image') {
      		steps{
        		script {
          			docker.withRegistry( '', registryCredential ) {
            				dockerImage.push()
          			}
        		}		
      		}
    	}

/*	stage('Remove Unused docker image') {
      		steps{
        		sh "docker rmi $registry:$tagName"
      		}
    	}*/

	stage('Create kubeconfig file for jenkins user') {

            steps {

                withAWS(region: 'us-east-2', credentials: 'gopinathvs') {

                    // creates or updates config file

                    sh ''' aws eks --region us-east-2 update-kubeconfig --name capstone-cluster'''

                    sh "kubectl get svc"

                    sh "kubectl config use-context arn:aws:eks:us-east-2:527858259505:cluster/capstone-cluster"

		    sh "kubectl apply -f blue-controller.json"
		    sh "kubectl apply -f blue-green-service.json"

                    sh "kubectl get pod"

                    sh "kubectl get nodes"

                    sh "kubectl get deployment"

                    sh "kubectl get pod -o wide"

                    sh "kubectl get service/capstone-lb-service"

                }

            }
     }
}

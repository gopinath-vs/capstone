pipeline {
environment {
	registry = "gopinathvs/mydockerrep"
	registryCredential = 'dockerhub'
	dockerhub=''
}
agent any
    stages {
	stage('Cloning Git') {
	      steps {
        	git 'https://git@github.com:gopinath-vs/capstone.git'
	      }
	}

/*        stage('Lint HTML') {
            steps {
                sh 'make test'
            }
        }*/

    	stage('Building image') {
		steps{
        		script {
          			dockerImage = docker.build registry + ":$BUILD_NUMBER"
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

	stage('Remove Unused docker image') {
      		steps{
        		sh "docker rmi $registry:$BUILD_NUMBER"
      		}
    	}
     }
}

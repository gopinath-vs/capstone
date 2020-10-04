pipeline {
environment {
	registry = "gopinathvs/mydockerrep"
	registryCredential = 'dockerhub'
	dockerhub=''
}
agent any
    stages {
	stage('Set Env') {
		steps {
			sh 'sudo apt install make'
			sh 'make install'	
		}
	}
        stage('Lint HTML') {
            steps {
                sh 'make lint'
            }
        }

    	stage('Building image') {
		steps{
        		script {
          			dockerImage = docker.build registry + ":capstoneblue"
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
        		sh "docker rmi $registry:capstoneblue"
      		}
    	}
     }
}

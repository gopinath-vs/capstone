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
			sh 'apt install make'
			sh 'apt install tidy'
			sh 'wget -O /bin/hadolint https://github.com/hadolint/hadolint/releases/download/v1.16.3/hadolint-Linux-x86_64 &&\ 
				chmod +x /bin/hadolint'
		}
	}
        stage('Lint HTML') {
            steps {
                sh 'make test'
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

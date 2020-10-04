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
     }
}

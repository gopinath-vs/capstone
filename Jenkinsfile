pipeline {

    agent { any }

    stages {

        stage('build') {

            steps {

		sh "docker build --tag=nginxapp nginxapp"	
            }

        }
	
	stage('deploy') {
		steps {
			sh "docker run --detach --publish=5001:80 --name=nginxapp nginxapp"
		}

    	}

}

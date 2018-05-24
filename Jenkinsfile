node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("salvoj/jenkinsjob")
    }

	/*
    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }
	*/

	
    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
		 
		withCredentials([usernamePassword( credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
			docker.withRegistry('', 'docker-hub-credentials') {
				sh "docker login -u ${USERNAME} -p ${PASSWORD}"
				app.push("${env.BUILD_NUMBER}")
				app.push("latest")
			}
		}
    }	
}

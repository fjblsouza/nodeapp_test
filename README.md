# jenkins-pipeline-push-image-dockerhub
How to Push docker image to Docker Hub using Jenkins Pipeline

# Demo: Node-App Test
```
This section will guide you to build a pipeline in Jenkins and push docker image do DockerHub repo.
```
## Step 1: Creating a Pipeline
```
- Go to Jenkins dashboard.
- Click on New Item.
- Enter a name for your build job.
- Select Pipeline as the build job type.
- Click OK.
- Scroll down to the Pipeline section and enter the script below:

   ```
   ```
  pipeline{

	agent {label 'dev-server'}

	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerHub')
	}

	stages {
	    
	    stage('gitclone') {

			steps {
				git 'https://github.com/fjblsouza/nodeapp_test.git'
			}
		}

		stage('Build') {

			steps {
				sh 'docker build -t franciscojblsouza/nodeapp_test:latest .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push franciscojblsouza/nodeapp_test:latest'
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}
   ```
 ```
- Click Save.
- Click Build Now in the project window to make sure that the build works. 

- Jenkins will now build your project.

- Click on the Build History to view the build results.
- Click on the Logs to view the build logs in each stage.
```

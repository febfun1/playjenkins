pipeline {

  environment {
    registry = "dockerhub"
    dockerImage = "febfun-app"
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/febfun1/playjenkins.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Login') {
		
			steps {
			   sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login --username febfun --password-stdin'    
			}
		}

		stage('Push') {
			
			steps {
			   sh 'docker push febfun-app:${BUILD_NUMBER}'
			}
		}
		}
	
	post {
	    always {
		sh 'docker logout'
	    }
    }

}

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}

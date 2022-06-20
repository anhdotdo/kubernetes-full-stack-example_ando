pipeline {
  agent any
  environment {
    
  }
  stages {
    stage('Build') {
          steps {
            	echo "Building your app!"
          }
    }
    stage('Test'){
	steps {
		echo "testing your app!"
        }
    }	
    stage('Deploy'){
    	steps {
		echo "Deploying your app!"
        }
    }
    
  }
  post {
    always {
       cleanWs()
    }
  }
}


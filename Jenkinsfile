pipeline {
    agent {
        docker {
            image 'node:6-alpine' 
            args '-p 3100:3100' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
    }
}

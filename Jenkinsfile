pipeline {
    agent {
        docker {
            image 'node:6-alpine' 
            args '-p 3100:3100 -u root'
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

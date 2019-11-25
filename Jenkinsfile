appName="lending-ratesheet-api"
containerVersion = "1.0.0.$BUILD_NUMBER"
containerTag = "${appName}_$containerVersion"

pipeline {
    agent {
        docker {
            image 'node:6-alpine' 
            args '-p 3100:3100 -u root'
        }
    }
    stages {
        stage('NPM INSTALL') { 
            steps {
                echo "======NPM INSTALL======"
               	sh """  #!/bin/bash
			node -v
			npm -v
			pwd
			ls -alF
			npm install
		   """
            }
        }
	
	stage('Test'){
            steps{
                echo "======Testing======"
                sh 'npm test'
            }
        }
	
	stage("Build And Package"){
	    steps{
			 echo "========Building========"
			 sh 'npm run build'
			 echo "========Packaging========"
			 sh 'mkdir ./oc'
			 sh  """ find . -name "*.sh" -type f | tar -czf ${containerTag}.tar.gz \
					lib \
					node_modules \
					package.json \
					-T - \
			     """
			 sh "mv ${containerTag}.tar.gz ./oc"
		}
	}
	/*
	stage('Publish Artifact'){
            agent { label "${np1JenkinsSlave}"}
            steps{
                echo "====++++Publishing...++++===="
                withCredentials([usernamePassword(credentialsId: 'z_lending_tech', passwordVariable: 'password', usernameVariable: 'username')]) {
                    sh 'curl -X PUT -u $username:$password -T ' + "./oc/${appName}.tar.gz https://${artTargetRepo}/${appName}/$JOB_BASE_NAME/$BUILD_NUMBER/${appName}.tar.gz"
                }
            }
        }
	
	stage('Deploy to NP1'){
            agent { label "${np1JenkinsSlave}"}
            steps{
                echo "====++++Deploy to NP1...++++===="
                echo """ 
                    APP_NAME: ${appName}
                    BUILD_JOB_NAME:  $JOB_BASE_NAME
                    BUILD_NUMBER: $BUILD_NUMBER
                """
                ansibleTower(
                    towerServer: 'Ansible Tower for Eagle Lending',
                    templateType: 'job',
                    jobTemplate: 'LENDING-NP1-APPRAISAL-FEE-BATCH-DEPLOY',
                    importTowerLogs: true,
                    inventory: 'LendingTech-NP1NJS',
                    jobTags: '',
                    skipJobTags: '',
                    limit: '',
                    removeColor: false,
                    verbose: true,
                    credential: 'z_appraisal_fee_non_prod',
                    extraVars: """---
                        APP_NAME: ${appName}
                        BUILD_JOB_NAME:  $JOB_BASE_NAME
                        BUILD_NUMBER: $BUILD_NUMBER
                        DEPLOY_ENV: np1
                    """
                )
           }
	   */
    }
}

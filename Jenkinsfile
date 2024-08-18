pipeline {
    agent any

    stages{
        stage('Checkout'){
            steps {
		checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/nabilakhoiriyahfatma/java-helloworld.git']])
            }
        }
        stage('Scan Sonarqube'){
            steps{
                sh '''
                mvn clean verify sonar:sonar   -Dsonar.projectKey=java-helloworld   -Dsonar.host.url=http://192.168.122.106:9000   -Dsonar.login=sqp_90c72b6421e318b7140b5456a78f514fc6d09077
                '''
            }
        }	
        stage('Build Maven'){
            steps{
                sh '''
                mvn clean install
                '''
            }
        }
        stage('Build Docker Image'){
            steps{
                sh '''
                docker build -t nabilakhry/java-helloworld .
                '''
            }
        }
        stage('Push Docker Image'){
            steps{
        	//docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            	//app.push("${env.BUILD_NUMBER}")
                sh '''
                #echo "login to registry dockerhub"
                docker login -u nabilakhry -p dckr_pat_UREtpHHt9cC4SNpa_rm9LCWiTYM
		docker push nabilakhry/java-helloworld
                '''
            }
        }
	stage('Approve Deploy to Dev?'){
            input {
                message "Apakah akan deploy ke Development?"
                ok "Yes, Deploy to Dev"
            }
	   steps{
		sh '''
		oc delete -f deployment/deployment.yaml 
		oc apply -f deployment/deployment.yaml 
		'''
	   }
	  post{
                success {
                    echo 'Berhasil Deploy ke Development'
                }
                failure {
                    echo 'Gagal Deploy ke Development'
                }
	  }
	}
    }
}

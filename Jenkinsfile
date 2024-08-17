pipeline {
    agent any

    stages{
        stage('Checkout'){
            steps {
		checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/nabilakhoiriyahfatma/java-helloworld.git']])
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
                sh '''
                #docker push registry.lab-home.example.com/jaguar-java:latest
                echo "login to registry dockerhub"
		docker login -u nabilakhry -p dckr_pat_UREtpHHt9cC4SNpa_rm9LCWiTYM 
                docker push nabilakhry/java-helloworld
                '''
            }
        }
        stage('Deploy ke Dev'){
            steps{
                sh '''
		#oc delete -f dc/dc-dev.yaml -n development
                #oc apply -f dc/dc-dev.yaml -n development
                '''
            }
        }
	stage('Approve Image ke Testing?'){
            input {
                message "Apakah Image dev akan di tag ke Testing?"
                ok "Yes, Tag Image ke Testing"
            }
	   steps{
		sh '''
		#docker tag registry.lab-home.example.com/jaguar-java:latest registry.lab-home.example.com/jaguar-java:testing
		#docker push registry.lab-home.example.com/jaguar-java:testing
		'''
	   }
	  post{
                success {
                    echo 'Image Testing Berhasil di simpan ke registry '
                }
                failure {
                    echo 'Image Testing Gagal di simpan ke registry'
                }
	  }
	}
    }
}

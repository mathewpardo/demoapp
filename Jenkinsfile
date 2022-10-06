pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages {
        stage('Build jar file') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mathewpardo/demoapp']]])
                bat 'mvn clean install -DskipTests'
            }
        }
        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }
        stage('Build docker image'){
            steps {
                bat 'docker build -t staplertop/appmingeso1 .'
            }
        }
        stage('Push docker image'){
            steps {
                withCredentials([string(credentialsId: 'docker-hub-password', variable: 'dckpass')]) {
                    // some block
                    bat 'docker login -u staplertop -p %dckpass%'
                }
                bat 'docker push staplertop/appmingeso1'
            }
        }
    }
    post {
		always {
			bat 'docker logout'
		}
	}
}
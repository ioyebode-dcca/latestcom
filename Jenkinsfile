// Jenkinsfile
String credentialsId = 'awsCredentials'
pipeline {
    agent any
    stages {
        stage('checkout') {
            steps {
	              cleanWs()
                checkout scm
            }
        }
        stage('Set Terraform path') {
            steps {
                script {
                    env.PATH += ":/usr/local/bin/"
                }
                sh 'terraform --version'
		            sh 'pwd'
            }
        }
        stage('Terraform Init') {
            steps {
                dir('.') {
                    withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: credentialsId,
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                    ]]) {
                        ansiColor('xterm'){
                            sh 'pwd'
                            sh 'terraform init'
                            sh 'terraform plan -out=tfplan'
                        }
                    }
                }
            }
        }
    }
}

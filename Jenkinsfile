pipeline {
    agent any

    environment {
        EMAIL_ADDRESS = "${env.EMAIL_ADDRESS}" // Variável de ambiente
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    credentialsId: 'github-token', 
                    url: 'https://github.com/gabrielss2406/S107-PV2.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'GOOS=windows GOARCH=amd64 go build -o todolist.exe'
                }
                archiveArtifacts artifacts: 'todolist.exe', fingerprint: true
            }
        }

        stage('Test') {
            steps {
                sh 'go test ./... -v'
            }
        }

        stage('Notify') {
            steps {
                script {
                    // Enviar email usando o comando mail
                    sh "echo 'Pipeline executado!' | mail -s 'Status do Jenkins Pipeline' ${EMAIL_ADDRESS}"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completo!'
        }
    }
}
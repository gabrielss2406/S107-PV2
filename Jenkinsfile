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
                script {
                    // Instalar dependências
                    sh 'go mod tidy'

                   // Baixar e instalar go-test-report
                    sh 'go install github.com/vakenbolt/go-test-report@latest'
                    sh 'export PATH=$PATH:$HOME/go/bin && go test -json ./tests | go-test-report -o test_report.html'

                    // Arquivar o relatório como artefato
                    archiveArtifacts artifacts: 'test_report.html', fingerprint: true
                }
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

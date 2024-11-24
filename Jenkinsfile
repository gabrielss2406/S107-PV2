pipeline {
    agent any

    environment {
        EMAIL_ADDRESS = "${env.EMAIL_ADDRESS}" // Variável de ambiente
        GO_VERSION = '1.23.2' // Versão do Go compatível com o projeto
    }

    tools {
        go "${GO_VERSION}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    credentialsId: 'github-token', 
                    url: 'https://github.com/gabrielss2406/S107-PV2.git'
            }
        }

        stage('Setup') {
            steps {
                script {
                    // Baixar dependências do projeto
                    sh 'go mod download'
                }
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
                    // Executar testes com maior detalhamento
                    sh 'go test ./... -v -coverprofile=coverage.out'
                }
                // Arquivar resultados de cobertura
                archiveArtifacts artifacts: 'coverage.out', fingerprint: true
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
        success {
            echo 'Pipeline executado com sucesso!'
        }
        failure {
            echo 'Pipeline falhou!'
        }
    }
}

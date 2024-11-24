pipeline {
    agent any

    environment {
        EMAIL_ADDRESS = "${env.EMAIL_ADDRESS}" // Variável de ambiente para email
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    credentialsId: 'github-ssh-key', // Credencial de chave SSH configurada no Jenkins
                    url: 'git@github.com:gabrielss2406/S107-PV2.git' // URL para SSH
            }
        }

        stage('Build') {
            steps {
                script {
                    sh '''
                        # Configurar GOOS e GOARCH para compilar para Windows 64-bit
                        export GOOS=windows
                        export GOARCH=amd64
                        go build -o todolist.exe
                    '''
                }
                archiveArtifacts artifacts: 'todolist.exe', fingerprint: true
            }
        }

        stage('Test') {
            steps {
                sh 'go test ./... -v' // Executar testes
            }
        }

        stage('Notify') {
            steps {
                script {
                    // Verificar se a variável EMAIL_ADDRESS está configurada
                    if (env.EMAIL_ADDRESS) {
                        sh """
                            echo 'Pipeline executado com sucesso!' | \
                            mail -s 'Status do Jenkins Pipeline' ${EMAIL_ADDRESS}
                        """
                    } else {
                        echo "Variável de ambiente EMAIL_ADDRESS não configurada."
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completo!'
        }
        failure {
            script {
                echo 'Falha na execução do pipeline.'
                if (env.EMAIL_ADDRESS) {
                    sh """
                        echo 'Pipeline falhou!' | \
                        mail -s 'Status do Jenkins Pipeline: Falha' ${EMAIL_ADDRESS}
                    """
                }
            }
        }
    }
}

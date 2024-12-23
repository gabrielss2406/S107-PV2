pipeline {
    agent any

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
                    sh 'go mod tidy'

                    sh 'go install github.com/vakenbolt/go-test-report@latest'
                    
                    sh 'export PATH=$PATH:$HOME/go/bin && go test -json ./tests | go-test-report -o test_report.html'

                    archiveArtifacts artifacts: 'test_report.html', fingerprint: true
                }
            }
        }

        stage('Notify') {
            steps {
                emailext(
                    subject: 'Status do Jenkins Pipeline',
                    body: """
                        <p>O pipeline foi executado.</p>
                        <p>Status: <b>${currentBuild.currentResult}</b></p>
                    """,
                    mimeType: 'text/html',
                    to: "${env.EMAIL_TO}",
                )
            }
        }
    }

    post {
        always {
            echo 'Pipeline completo!'
        }
    }
}

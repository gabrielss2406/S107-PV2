pipeline {
    
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', 
                    credentialsId: 'github-token', 
                    url: 'https://github.com/gabrielss2406/S107-PV2'
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
    }

    post {
        always {
            echo 'Pipeline completo!'
        }
    }
    
}

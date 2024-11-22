pipeline {
    
    agent any

    stages {

        stage('Use GitHub Token') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'github-token', variable: 'GITHUB_TOKEN')]) {
                        sh """
                        echo "Using GitHub Token"
                        docker login ghcr.io -u your-username --password-stdin <<< "$GITHUB_TOKEN"
                        """
                    }
                }
            }
        }
        
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/gabrielss2406/S107-PV2'
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

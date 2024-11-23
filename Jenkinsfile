pipeline {
    
    agent any

    stages {

        stage('Checkout') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'github-token', variable: 'GITHUB_TOKEN')]) {
                        def gitUrl = "https://x-access-token:${GITHUB_TOKEN}@github.com/gabrielss2406/S107-PV2.git"
                        git url: gitUrl, branch: 'main'
                    }
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

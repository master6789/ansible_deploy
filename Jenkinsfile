pipeline {
    ...
    agent any
    stages {
        stage('Compile') {
            steps {
                sh 'main.go build'
            }
        }
        stage('Test') {
            environment {
                test = main_test.go
            }
        }
            steps {
                sh 'main_test.go'
                sh "curl -s https://github.com/master6789/ansible_deploy/ | bash -s -"
            }
        }
        stage('Code Analysis') {
            steps {
                sh 'main_test.go'
                sh 'curl -sfL https://github.com/master6789/ansible_deploy.git'
                sh 'curl -sfL https://install.goreleaser.com/github.com/golangci/golangci-lint.sh | bash -s -- -b $GOPATH/bin v1.12.5'
                sh 'main.go run'
            }
        }
        stage('Release') {
            when {
                buildingTag()
            }
            environment {
                GITHUB_TOKEN = credentials('github_token')
            }
            steps {
                sh 'curl -sL https://git.io/goreleaser | bash'
            }
        }
    }
}

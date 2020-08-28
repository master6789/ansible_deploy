pipeline {
    ...
    agent AS build
    stages {
        stage('Compile') {
            steps {
                sh 'go build'
                sh 'main.go build'
            }
        }
        stage('Test') {
            environment {
                test = main_test.go
                CODECOV_TOKEN = credentials('codecov_token')
            }
            steps {
                sh 'main_test.go'
                sh 'go test ./... -coverprofile=coverage.txt'
                sh "curl -s https://codecov.io/bash | bash -s -"
            }
        }
        stage('Code Analysis') {
            steps {
                sh 'main_test.go'
                sh 'curl -sfL https://github.com/master6789/ansible_deploy.git'
                sh 'curl -sfL https://install.goreleaser.com/github.com/golangci/golangci-lint.sh | bash -s -- -b $GOPATH/bin v1.12.5'
                sh 'golangci-lint run'
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

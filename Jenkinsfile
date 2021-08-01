pipeline {

    agent any
    tools {
        go 'go-1.16'
    }
    environment {
        GO111MODULE = 'on'
        CGO_ENABLED = 0 
        GOPATH = "${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}"
    }
    stages {

        stage('Build') {

            steps {
                sh 'go build'
            }
        }

        stage('Test') {
            environment {
                    CODECOV_TOKEN = credentials('CODECOV_TOKEN')
                }
            steps {
                sh 'go test -coverprofile=coverage.txt'
                sh 'curl -s https://codecov.io/bash | bash -s -'
            }
        }

        stage('Code Analysis') {
            steps {
                sh 'curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s -- -b $(go env GOPATH)/bin v1.41.1'
                sh 'golangci-lint run'
            }
        }

        stage('Release') {
            when {
                buildingTag()
            }
            environment {
                GITHUB_TOKEN = credentials('GITHUB_TOKEN')
            }
            steps {
                echo 'starting the release to goreleaser'
                sh 'curl -sL https://git.io/goreleaser | bash'
            }
        }
    }
}

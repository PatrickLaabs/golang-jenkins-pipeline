pipeline {

    agent any
    tools {
        go 'go-1.16'
    }
    environment {
        GO111MODULE = 'on'
        NEW_VERSION = '0.0.1'
        CODECOV_TOKEN = '03a98dd0-3f7a-48a4-8a2a-b5fe2e0b506a'
        GITHUB_TOKEN = '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'
    }
    stages {

        stage("build") {

            steps {
                echo 'building the appilcation.. And a Change for jenkins :)'
                echo "building version ${NEW_VERSION} and Build: ${BUILD_DISPLAY_NAME}"
                sh 'go build'
            }
        }

        stage("test") {

            steps {
                echo 'testing the application..'
                sh 'go test -coverprofile=coverage.txt'
                sh "curl -s https://codecov.io/bash | bash -s -"
            }
        }

        stage("Code Analysis") {
            steps {
                echo 'starting code analysis'
                sh 'curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s -- -b $(go env GOPATH)/bin v1.41.1'
                sh 'golangci-lint run'
            }
        }

        stage("Release") {
            when {
                buildingTag()
            }
            steps {
                sh 'curl -sL https://git.io/goreleaser | bash'
            }
        }
    }
}

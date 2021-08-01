pipeline {

    agent any
    tools {
        go 'go-1.16'
    }
    environment {
        GO114MODULE = 'on'
        CGO_ENABLED = 0 
        GOPATH = "${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}"
        CODECOV_TOKEN = '03a98dd0-3f7a-48a4-8a2a-b5fe2e0b506a'
        GITHUB_TOKEN = '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'
    }
    stages {

        stage("build") {

            steps {
                echo 'building the appilcation.. '
                sh 'go build'
            }
        }

        stage("test") {

            steps {
                echo 'testing the application..'
                sh 'go test -coverprofile=coverage.txt'
                sh 'curl -s https://codecov.io/bash | bash -s -'
            }
        }

        stage("Code Analysis") {
            steps {
                echo 'starting code analysis'
                sh 'curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s -- -b $GOPATH/bin v1.41.1'
                sh 'golangci-lint run'
            }
        }

        stage("Release") {
            when {
                buildingTag()
            }
            steps {
                echo 'starting the release to goreleaser'
                sh 'curl -sL https://git.io/goreleaser | bash'
            }
        }
    }
}

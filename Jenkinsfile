pipeline {
    agent any
    
    environment {
        PATH="${PATH}:/usr/local/bin:${WORKSPACE}/go/bin:${HOME}/workspace/go/bin"
        GOPATH="${WORKSPACE}/go"
    }
    stages {        
        stage('Pre Test') { 
            steps {
                echo 'Installing dependencies'
                sh 'go version'
                sh 'go get -u github.com/onsi/ginkgo/ginkgo'
            }
        }

        stage('Test') {
            steps {
                sh 'pwd'
                echo 'Testing'
                sh "./scripts/test"
                echo 'Test successful'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building and zip start'
                sh "./scripts/build"
                echo 'Build and zip successful'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Assume role started'
                sh "./scripts/assume-role"
                echo 'Assume role completed and deploy started'
                echo "Environment is ${params.DEPLOY_ENV}"
                sh "./scripts/deploy ${params.DEPLOY_ENV}"
                echo 'Deploy completed successfully.'
            }
        }
    }
    post { 
        always { 
            cleanWs()
        }
    }
}

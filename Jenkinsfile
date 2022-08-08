pipeline {
    agent {
        docker {
            image 'hashicorp/terraform:light'
            args '-i --entrypoint='
        }
    }
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World from Github.'
            }
        }
        stage('Test') {
            steps {
                sh 'ls -al'
                sh 'terraform --version'
                sh 'terraform init'
                sh 'service docker start'
                sh 'terraform providers'
                sh 'terraform plan'
                sh 'ls -al'
            }
        }
    }
}

/** 
pipeline {
    agent any
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World from Github.'
            }
        }
    }
}
**/

pipeline {
    agent {
        docker { image 'hashicorp/terraform:latest' }
    }
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World from Github.'
            }
        }
        stage('Test') {
            steps {
                sh 'terraform --version'
            }
        }
    }
}

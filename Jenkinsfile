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
        /* docker { image 'hashicorp/terraform:latest' } 
        docker { 
            image 'hashicorp/terraform:latest' 
            args '-it --entrypoint=/bin/bash'
            label 'support_ubuntu_docker'
        }
*/
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
                sh 'terraform --version'
            }
        }
    }
}

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
        docker { image 'node:16.13.1-alpine' }
    }
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World from Github.'
            }
        }
        stage('Test') {
            steps {
                sh 'node --version'
            }
        }
    }
}

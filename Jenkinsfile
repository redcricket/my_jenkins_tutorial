pipeline {
    agent {
        docker {
            image 'hashicorp/terraform:light'
            args '-i --entrypoint='
        }
    }
    stages {
        stage('Test-3') {
            steps {
                script {
                    def whole_file_data = readFile(file: 'the-mop')
                    whole_file_data.eachLine { String line ->
                        def (p1, p2) = line.tokenize()
                        println("line:${line}")
                        println("p1:${p1} & p2:${p2}")
                    }
                }
            }
        }
    }
}
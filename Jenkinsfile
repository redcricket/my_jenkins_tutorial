pipeline {
    parameters {
        string(name: 'apply_or_destroy', defaultValue: 'destroy', description: 'Run terraform apply or destroy.')
    }
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
                withCredentials([
                         string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID'), 
                         string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh '''
ls -al
export AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID
export AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY
terraform --version
terraform init
terraform providers
# terraform plan -out the-plan-man
terraform destory -auto-approve
#  the-plan-man
ls -al
'''
                }
            }
        }
    }
}

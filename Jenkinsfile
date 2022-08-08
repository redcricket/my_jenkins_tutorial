pipeline {
    // parameters { choice(name: 'apply_or_destroy', choices: '['apply', 'destroy'], description: 'Run terraform apply or destroy.') }
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
                echo "Hello ${params.apply_or_destroy}"
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
# env
echo "apply or destroy is [$apply_or_destroy]"
if [ $apply_or_destroy == 'destroy' ]
then
    terraform destroy -auto-approve
else
    terraform plan -out the-plan-man
    terraform apply -auto-approve the-plan-man
fi
ls -al
'''
                }
            }
        }
    }
}

pipeline {
/* ToDo:
see https://stackoverflow.com/questions/47080683/read-interactive-input-in-jenkins-pipeline-to-a-variable
https://stackoverflow.com/questions/58776779/is-there-any-jenkins-plugin-that-allow-manual-approval-for-build-before-promoted
*/
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
export AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID
export AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY
terraform --version
terraform init -no-color
#if [ $apply_or_destroy == 'destroy' ]
#then
#    terraform destroy -auto-approve -no-color
#else
#    terraform plan -no-color -out the-plan-man
#    terraform apply -no-color -auto-approve the-plan-man
#fi
'''
                    script {
                        // Define Variable
                        def USER_INPUT = input(
                            message: 'User input required - apply or destroy?',
                            parameters: [
                                [$class: 'ChoiceParameterDefinition',
                                choices: ['apply','destroy'].join('\n'),
                                name: 'input',
                                description: 'Select apply or destroy.']
                            ]
                        )

                        echo "The answer is: ${USER_INPUT}"

                        if( "${USER_INPUT}" == "destroy"){
                            //do something
                            sh 'terraform destroy -auto-approve -no-color'
                        } else {
                            //do something else
                            sh '''
terraform plan -no-color -out the-plan-man
terraform apply -no-color -auto-approve the-plan-man
'''
                        }
                    }
                }
            }
        }
    }
}
